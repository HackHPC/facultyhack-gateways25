## Quick orientation for AI coding agents

This repository is a Jekyll static site (theme: `jekyll-theme-yat`) used to host the FacultyHack@Gateways25 event site. The goal of this file is to give an AI coding agent the exact, actionable knowledge needed to be productive editing content, data, templates, styles, and builds.

Key facts
- Build system: Ruby + Bundler + Jekyll (see `Gemfile` and `Gemfile.lock`). Project uses Jekyll 4.x and is bundled with Bundler 2.4.x.
- Base URL: `baseurl` is set in `_config.yml` (site is published under `/facultyhack-gateways25`). Use `{{ "..." | relative_url }}` everywhere assets or links are needed.
- Generated output lives in `_site/` — do not edit files under `_site` (they are generated).

Where to look (high-value files)
- `_config.yml` — site-wide configuration (plugins, baseurl, theme options).
- `Gemfile`, `Gemfile.lock` — Ruby deps and exact Jekyll/plugin versions.
- `_data/teams.yml` — authoritative data for the Teams page. Note structure: top-level `teams:` array; each team uses keys like `teamname`, `mentor`, `members`, `coursedescription` and `projectgoals`. Some fields contain HTML (multi-line YAML using `| -`), so preserve HTML formatting when editing.
- `_includes/teams.html` — Liquid template that renders `site.data.teams.teams`. It relies on parallel arrays for `members.names`, `members.socials`, `members.email` and matches entries by `forloop.index0`. When editing or adding new fields, keep the index-aligned arrays consistent.
- `_layouts/default.html` and `_includes/head.html` — overall page layout and where CSS/JS are included. To add global assets, update `head.html` to include them and add files under `assets/`.
- `_sass/` and `assets/css/main.css` — styles; theme partials live under `_sass/yat/` and project-specific overrides under `_sass/teams/`.

Conventions and gotchas (do not assume defaults)
- Data arrays are often index-aligned (example: `team.members.names`, `team.members.socials`, `team.members.email`) and the templates use `forloop.index0` to pair them. If you convert these to an array-of-objects, update the Liquid template accordingly.
- Many YAML descriptions include HTML (see `coursedescription` in `_data/teams.yml`). Preserve HTML escaping and formatting; Liquid prints raw HTML by default when the value contains HTML.
- External vs. internal links: templates check `if link.url contains '://'` to decide whether to treat a link as external. Respect that convention when adding `links` entries.
- Use `| relative_url` in templates to respect `baseurl` — assets and links in code use `relative_url` filters extensively.

Common tasks & exact commands (macOS zsh)
- Install dependencies (first time / CI):
  - bundle install
- Build the site locally (single build):
  - bundle exec jekyll build
- Serve with live reload during edits:
  - bundle exec jekyll serve --livereload
- If assets appear broken locally, confirm `_config.yml` `baseurl` and that templates use `relative_url`.

File edit examples (copyable intent)
- Add a new team in `_data/teams.yml` as another item under `teams:`. Keep member arrays aligned: `members.names`, `members.socials`, `members.email`.
- To change how member contact icons are rendered, edit `_includes/teams.html` — look for the block that checks `team.members.socials[forloop.index0]` and `team.members.email[forloop.index0]`.
- To add a site-wide stylesheet, add the CSS file under `assets/css/` and include it in `_includes/head.html` (it already includes `assets/css/main.css`).

Testing and verification
- After edits, run `bundle exec jekyll build` and inspect `_site/` for the rendered output (or run `serve` with `--livereload`).
- For data changes in `_data/teams.yml`, open `teams.html` (rendered page in `_site/teams.html`) and search for the new `teamname` anchor (templates render an id using `{{ team.teamname }}`).

When to change templates vs data
- Prefer changing `_data/*.yml` for content edits (teams, schedule, organizers). Change `_includes/*.html` or `_layouts/*.html` when you need structural or presentation changes.

Developer notes for maintainers
- This repo uses several Jekyll plugins declared in `_config.yml` (jekyll-feed, jekyll-seo-tag, jekyll-sitemap, etc.). Local builds must run under the same Bundler/Gem versions to avoid differences.
- The theme is `jekyll-theme-yat` (see `jekyll-theme-yat.gemspec`); much of the structure comes from that theme. When modifying theme behavior, prefer overrides in `_layouts`, `_includes`, or `_sass` rather than editing the gemspec.

If something looks wrong
- Check `_site/` to see what was generated and whether the Liquid scaffolding produced the expected HTML.
- Look in `_config.yml` for global toggles (e.g., `header_pages`, `night_mode`, `brand_color`) that change rendering.

If anything here is unclear or you want the agent to follow stronger rules (e.g., always add tests or open PR templates), tell me what to include and I'll update this file.
