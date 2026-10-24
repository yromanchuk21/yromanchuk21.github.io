# Subtle Hacker — GitHub Pages portfolio

A project-first, dark Jekyll portfolio inspired by the official [pages-themes/hacker](https://github.com/pages-themes/hacker) theme. It keeps the terminal-influenced labels and code-friendly feel while using a desaturated green palette and readable system sans-serif body text.

## What is included

- Project-first home page with Projects, Cybersecurity Notes / HTB Write-ups, Scripts, Reverse Engineering, and About sections
- Responsive cards and mobile navigation
- Jekyll collections for project pages, scripts, and reverse-engineering notes
- Accessible skip link, semantic landmarks, visible focus-capable controls, and reduced-motion support
- Rouge syntax highlighting through GitHub Pages/Jekyll
- Clearly labelled placeholder content only—no invented biography or accomplishments
- A GitHub Actions Pages deployment workflow

## Local preview

1. Install Ruby and Bundler.
2. From this folder, run `bundle install`.
3. Run `bundle exec jekyll serve`.
4. Open the address printed in the terminal (normally `http://localhost:4000`).

## Customize

1. Set your name, email, description, GitHub Pages URL, and `baseurl` in `_config.yml`.
2. Replace the introduction in `index.md` and the placeholder text in `about.md`.
3. Add or replace files in `_projects/`, `_scripts/`, `_reversing/`, and `_posts/`. The front matter fields used by cards are `title`, `kind`, `summary`, and `tags`.
4. Adapt colors and typography in `assets/css/style.css`. The CSS variables at the top are the simplest place to start.
5. Delete the example files once your actual content is ready.

## Deploy on GitHub Pages

1. Create a repository named `your-github-username.github.io` for a user site, or choose any repository for a project site.
2. Upload these files, commit, and push them to the repository's `main` branch.
3. In GitHub: **Settings → Pages → Build and deployment**, choose **GitHub Actions** as the source.
4. The included workflow deploys after each push to `main`. Wait for the workflow to complete, then open the URL shown in the Pages settings.

For a project site, set `baseurl: "/repository-name"` in `_config.yml`. For a user site, leave it empty.

## Notes

This is a custom, standalone theme inspired by Hacker; it does not pull a remote theme at build time. Review all published research for authorization, sensitive details, and disclosure requirements.
