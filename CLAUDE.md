# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static marketing site for Timberly, served by GitHub Pages at https://timber.to (custom domain set in `CNAME`; don't delete or rename it — it controls the Pages domain). The product app itself lives elsewhere (`https://app.timber.to`, linked from the nav and CTAs).

There is no build step, package manager, linter or tests. Every page is a self-contained HTML file with inline `<style>` and `<script>`; pushing to `main` deploys. To preview locally, serve the repo root (e.g. `python3 -m http.server`) so root-relative links like `/privacy-policy.html` resolve.

## Structure

- `index.html` — landing page: hero with animated WhatsApp chat, "para quién", a sticky-phone scroll story (log → intake → cut → drying → grading → dispatch), dashboard mock, modules grid, final CTA, footer. Product UI is mocked in inline HTML/SVG; photos come from `images/`. Final CTA links to WhatsApp (`wa.me/5493764705749`, prefilled message from the `wa_msg` key) and `mailto:leandro@timber.to` (subject from `mail_subject`); both hrefs are rewritten in `setLang()`.
- `privacy-policy.html`, `terms-of-service.html` — standalone legal pages, English only, with their own copy of the CSS variables/nav styles (no shared stylesheet, so style changes must be duplicated across files).

## i18n on the landing page

`index.html` supports EN / ES / PT entirely client-side:

- Text elements are empty in the markup and carry `data-i18n="key"` (set as `textContent`) or `data-i18n-html="key"` (set as `innerHTML`, used where the copy contains markup such as the hero title).
- All copy lives in the `T` object in the inline script at the bottom of `index.html`, one sub-object per language. **Any copy change must be made in all three languages**, and any new `data-i18n` key must be added to every language (missing keys render as empty).
- `detectLang()` picks the language from `localStorage.lang`, then `navigator.language`, defaulting to `en`. `setLang()` also rewrites `document.title` and the meta description from `hero_slogan`.

Animations (hero chat loop, scroll reveals, chapter-driven phone via IntersectionObserver) are CSS + vanilla JS and are disabled under `prefers-reduced-motion`.
