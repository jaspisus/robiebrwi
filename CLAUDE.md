# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Communication

Always communicate with the user in **English**, even though the site content is in Polish. Keep the user-facing page copy in Polish (`<html lang="pl">`); everything else (chat, code comments, commit messages, docs) is in English.

## What this is

A single-page static landing site for "robię brwi" (Wiktoria Jackowska), an eyebrow styling business. It acts as a link hub: Instagram, Booksy booking, Google Maps directions and a contact email.

There is no build step, package manager, linter or test suite. The deployable site is just the `www/` directory:

- `www/index.html` holds everything: markup, inline `<style>` and inline SVG icons. There is no JS.
- `www/favicon.svg` is a plain circle in the page background color.

## Local preview

```sh
python3 -m http.server -d www 8000   # then open http://localhost:8000
```

## Deployment

`.github/workflows/deploy.yml` deploys on every push to `main` (and on manual `workflow_dispatch`). It mirrors `www/` to the server over SFTP with `lftp mirror --reverse`. Things to keep in mind:

- **Pushing to `main` deploys to production immediately.** Work on a branch and merge when ready.
- Only files inside `www/` are published. Anything outside it (including this file) stays in the repo only.
- The mirror runs **without `--delete`**, so renaming or removing a file in `www/` leaves the old copy on the server.
- The workflow needs the `FTP_SERVER`, `FTP_USERNAME`, `FTP_PASSWORD` and `SSH_KNOWN_HOSTS` secrets and the `FTP_SERVER_DIR` repo variable.

## Design conventions

- Colors are CSS custom properties on `:root` (`--bg` `#403632`, `--fg` `#d3d0cb`, plus muted/line/hover variants that are `--fg` at lower alpha). If `--bg` changes, also update `<meta name="theme-color">` and the fill in `favicon.svg`.
- The only font is Cormorant Garamond from Google Fonts, with serif fallbacks.
- Mobile-first layout: a single centered column capped at `max-width: 440px`, fluid type via `clamp()`, and safe-area insets for padding.
- Link icons are inline 24×24 stroke SVGs that inherit `currentColor` and get their styling from `.link svg`. New links should copy the existing `<li><a class="link">` pattern, using `target="_blank" rel="noopener"` for external URLs.
- CSS class names follow a light BEM style (`logo__title`, `logo__subtitle`).
