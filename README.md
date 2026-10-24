# Industrial Cyberpunk Portfolio for GitHub Pages

A project-first Jekyll portfolio built with [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes). The visual system is intentionally restrained: graphite surfaces, muted teal signals, strong type, readable code, and no decorative "hacker" effects competing with the work.

> The bundled projects, research entries, and notes are **clearly labeled examples**. They are prompts to replace, not claims about a real person or portfolio.

## Quick start

1. Create a GitHub repository. For a user site, name it `YOUR-USERNAME.github.io`; otherwise use any repository name.
2. Copy this template into the repository root.
3. In `_config.yml`, replace `YOUR-USERNAME`, `YOUR-REPOSITORY`, the title, description, and footer link. For a user site, set `baseurl: ""`.
4. Commit and push to the `main` branch.
5. On GitHub, go to **Settings → Pages → Build and deployment** and select **GitHub Actions**. The included workflow will build and deploy the site.

GitHub Actions publishes on every push to `main`. The first deployment can take a few minutes.

## Customize the content

| What | Where |
| --- | --- |
| Site identity, repository URL, collections | `_config.yml` |
| Navigation labels and links | `_data/navigation.yml` |
| Homepage introduction and project-first layout | `index.md` |
| Portfolio case studies | `_projects/` |
| Research entries | `_research/` |
| Short writing | `_notes/` |
| Bio and contact links | `_pages/about.md` |
| Industrial color and card styling | `assets/css/main.scss` |

### Add a project

Create `_projects/my-project.md`:

```yaml
---
title: "Your real project title"
excerpt: "A concise, accurate one-sentence summary."
featured: true
order: 4
category: "EMBEDDED SYSTEMS"
tags: [C, Hardware, Testing]
---

Describe the scope, your role, evidence, constraints, and links to public work.
```

Set `featured: true` to show it on the home page. Smaller `order` values appear first. Use Markdown code fences for code samples; Minimal Mistakes supplies syntax highlighting.

## Local preview (optional)

Install Ruby and Bundler, then run:

```sh
bundle install
bundle exec jekyll serve
```

Open the local address shown in the terminal. For a project site, Jekyll will serve beneath the `baseurl` set in `_config.yml`.

## Design tokens

- Graphite background: `#171B20`
- Muted teal accent: `#58B6AE`
- Panels and border lines are deliberately low contrast to keep focus on writing and project evidence.

## Deployment notes

The workflow in `.github/workflows/pages.yml` uses official GitHub Pages Actions and the gems in `Gemfile`. It needs no personal access token. If your default branch is not `main`, edit the `branches` value in that workflow.

## License

Use this template as a starting point for your own portfolio. Minimal Mistakes is released under the MIT License; see its repository for complete theme licensing information.
