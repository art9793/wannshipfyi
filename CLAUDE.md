# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

WannaShip.fyi is a static landing page selling a curated Notion database of 50 validated startup ideas with build/sell playbooks. $49 one-time purchase via Dodo Payments, delivered via email.

## Architecture

Zero-dependency static site deployed on GitHub Pages. No build step — push to `main` deploys automatically.

- **`index.html`** — Conversion-optimized landing page (~600 lines). Uses Tailwind CSS via CDN with inline config, Space Grotesk + JetBrains Mono from Google Fonts, and vanilla JS (~15 lines) for IntersectionObserver animations and sticky CTA.
- **`thank-you.html`** — Post-purchase confirmation with social share buttons (Twitter/LinkedIn) and testimonial request. Has `noindex` meta tag.
- **`styles.css`** — Custom keyframe animations (`float`, `fadeUp`), `.fade-up`/`.is-visible` scroll reveal classes, `.row-blur` for product preview curiosity gap, `.highlight-col` for comparison table.
- **`wannaship-complete-master-document.md`** — Internal product spec (gitignored). Contains 20 reference example ideas (for quality calibration), pricing rationale, and persona research. The product has 50 ideas total, sourced fresh from Starter Story. **Always cross-reference this file when editing landing page copy to avoid over-promising.**

## Key Conventions

- **Tailwind config** is inline in `<script>` tags in both HTML files. Custom colors: `accent` (#f59e0b), `accent-hover` (#d97706). Font families: `font-display` (Space Grotesk), `font-mono` (JetBrains Mono).
- **CTA buttons** all link to `https://checkout.dodopayments.com/buy/pdt_0NY8eop2ZlHBkDdATJQmb?quantity=1&redirect_url=https://wannaship.fyi%2Fthank-you` and have unique IDs for future analytics: `cta-header`, `cta-hero`, `cta-sticky`, `cta-after-preview`, `cta-after-ideas`, `cta-final`.
- **No external JS libraries.** All animations are CSS-only or minimal vanilla JS with IntersectionObserver.
- **Source credibility bar** uses inline SVGs for logos (Reddit, IndieHackers, YouTube, Starter Story, TrustMRR) — not image files.

## Security Rules

- **Never** put the Notion template link in the repo, HTML, or any committed file. Product is delivered only via email after purchase (Dodo webhook → Resend/AutoSend).
- The master document (`wannaship-complete-master-document.md`) is gitignored and must stay excluded.

## Copy Accuracy

Landing page copy must match what the product actually delivers. The master document is the source of truth. Key constraints:
- Not all 50 ideas will have verified MRR — some are "validated patterns" with demand signals but no revenue proof. Use "research-backed" not "revenue-verified" when referring to all ideas collectively.
- Specific numbers (subreddit counts, community counts, DM script counts) should only be used if they match the actual product contents.

## Notion Database Build

The product sold on this landing page is a Notion database of 50 startup idea playbooks. The database is built separately using a pipeline documented in `workflow-notion-db-build.md`.

- **Status:** 5 of 50 playbooks published (calibration batch). See `PROJECT-STATUS.md` for current state.
- **Pipeline:** Ideas sourced from Starter Story → playbooks generated → humanized → Notion markdown → uploaded
- **Staging files:** All intermediate data lives in `staging/` (see `staging/README.md`)
- The master document's 20 ideas are reference examples for quality calibration — all 50 database ideas are sourced fresh from Starter Story.
- Landing page copy must match what's actually in the database. Cross-reference before changing any product claims.

## SEO

- JSON-LD structured data (Product + Organization) in `index.html` `<head>`
- Open Graph + Twitter Card meta tags
- `robots.txt` blocks `/thank-you.html`, references `sitemap.xml`
- `sitemap.xml` includes only the landing page
- Canonical URL: `https://wannaship.fyi/`
