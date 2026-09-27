---
title: "Triaging Perforce replica lag before it becomes an outage"
tags: [perforce, ci-cd, debugging]
---

A recurring failure mode in large Helix Core deployments: a read-only replica
quietly falls behind the master, builds start reading stale data, and nobody
notices until CI failures spike. Here's the checklist I run before it turns
into a P1.

## 1. Confirm it's actually lag, not a network blip

```bash
p4 -p replica:1666 pull -lj
```

This shows the last journal counter the replica has applied. Compare it
against the master's current journal counter:

```bash
p4 -p master:1666 counter journal
```

A gap that isn't closing over a few polling intervals — not a single
noisy sample — is the real signal.

## 2. Check what's actually blocking replication

Usually one of three things:

- A long-running `p4 verify` or `p4 obliterate` holding a lock on the master
- Disk I/O saturation on the replica (journal replay is sequential and disk-bound)
- A single oversized changelist replaying slowly

`p4 pull -l` on the replica shows in-flight archive transfers; if the same
file transfer has been "in progress" for an unreasonable amount of time,
that's usually the culprit.

## 3. Don't just restart the replica

Restarting clears the symptom but not the cause, and if the root cause was
disk I/O, you'll be back here in a few hours. Pull the journal replay rate
from `p4 pull -lj` before and after any fix to confirm the lag is actually
closing, not just reset.

## 4. Make the failure loud, not silent

The real fix is usually upstream: a Jenkins job or sync step that should be
pointed at a replica but silently falls back to the master under load, or a
missing alert on `p4 pull -lj` drift. A lagging replica is a minor
inconvenience; a lagging replica nobody is watching is an outage waiting
for a Monday morning sync storm.

---

*This is a template post — swap in your own incident notes, command output,
and root causes as you write new entries. Drop new files in `_posts/` named
`YYYY-MM-DD-title.md` and they'll show up on the [blog](/blog/)
automatically.*
