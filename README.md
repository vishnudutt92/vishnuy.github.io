# vishnu-yadav.github.io

Personal portfolio + engineering blog, built with Jekyll and deployed to
GitHub Pages via GitHub Actions.

## 1. Push this to a new repo

```bash
cd site
git init
git add .
git commit -m "Initial portfolio + blog"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

Two repo-naming options:
- `<your-username>.github.io` → site is live at `https://<your-username>.github.io/`
- any other name, e.g. `portfolio` → site is live at
  `https://<your-username>.github.io/portfolio/`. In that case, set
  `baseurl: "/portfolio"` in `_config.yml` before pushing.

## 2. Turn on GitHub Pages via Actions

In the repo on GitHub: **Settings → Pages → Build and deployment → Source**,
select **GitHub Actions**. That's it — the workflow in
`.github/workflows/deploy.yml` builds the Jekyll site and deploys it on
every push to `main`. Check the **Actions** tab for build status; the live
URL appears there once the `deploy` job finishes.

## 3. Add a new blog post

Add a file to `_posts/` named `YYYY-MM-DD-your-title.md`:

```markdown
---
title: "Your post title"
tags: [jenkins, ci-cd]
---

Body in Markdown.
```

Push to `main` and it appears on `/blog/` automatically.

## 4. Customize

- `index.html` — hero, skills, experience, projects, contact
- `assets/css/style.css` — all styling, as CSS custom properties at the top
- `_config.yml` — site title, description, author email/LinkedIn
- `_layouts/` and `_includes/` — page structure and nav

## 5. Run it locally (optional)

```bash
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000`.
