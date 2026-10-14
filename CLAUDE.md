# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A documentation-only Jekyll site (GitHub Pages, `https://novaengine-admin.github.io`) for NovaEngine's Salesforce managed packages. Currently it documents **Geo Regions** (choropleth maps for Lightning pages) and its **Australia** country extension. There is no application code, build script, test suite, or linter in the repo; GitHub Pages builds and deploys on push to `main`.

## Related repo

This is the documentation repo for the Salesforce DX solution at `../AU_Map_Component` (sibling directory, namespace `novaengine`). It contains two packages: `packages/geo-regions` (core engine, "Geo Regions") and `packages/geo-regions-au` ("Geo Regions - Australia", depends on the core package). Package versions are defined in its `sfdx-project.json`; when a new package version is released there, update the install links/version tables here. That repo has its own `CLAUDE.md` and `docs/`; consult them for component API and map details before changing the docs.

## Structure

- `_config.yml` — uses the remote theme `just-the-docs/just-the-docs` via `jekyll-remote-theme`; search enabled, light color scheme.
- `_sass/custom/custom.scss` — NovaEngine styling overrides (colors defined as `--nova-*` CSS variables on `:root`). Just the Docs picks up `_sass/custom/custom.scss` automatically.
- `index.md` — site root (`nav_exclude: true`, `has_children: true`).
- `geo-regions/index.md` — "Geo Regions" top-level nav entry and subscriber overview (install, permission set, App Builder usage, troubleshooting).
- `geo-regions/developer-guide.md` — component API, config, events, examples (child of Geo Regions).
- `geo-regions/australia-extension/index.md` — Australia packages, install links, map table (child of Geo Regions, `has_children`).
- `geo-regions/australia-extension/<state>.md` — one **generated** page per map (key, notes, region names) built from the GeoJSON `LGA_NAME25` property in `../AU_Map_Component/packages/geo-regions-au/main/default/staticresources`; regenerate rather than hand-edit when maps change, and keep the map table in the extension index in sync.
- `README.md` — short pointer only; no product docs.

Child pages' `parent` (and `grand_parent`) must match the parent page `title` exactly.

## Conventions

- Pages are Markdown with Just the Docs front matter (`title`, `parent`, `nav_order`, `has_children`, `permalink`). New country/extension pages go in their own folder as `index.md` with an explicit `permalink`.
- Salesforce package install links (`04t...` IDs) appear in a table showing only the latest released version, with Production (`login.salesforce.com`) and Sandbox (`test.salesforce.com`) variants; update both and the version/release columns together on each release.

## Previewing locally (optional, not set up in repo)

No Gemfile exists. To preview you would need Ruby/Jekyll with `jekyll-remote-theme` (network access to fetch the theme); otherwise verify changes by pushing and checking the GitHub Pages build.
