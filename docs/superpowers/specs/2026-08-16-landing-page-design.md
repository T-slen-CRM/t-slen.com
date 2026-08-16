# T-Slen CRM landing page — design spec

Date: 2026-08-16

## Purpose

A single-page marketing site for T-Slen CRM, hosted via GitHub Pages at the
custom domain `t-slen.com`. Primary audience: SMB business owners evaluating
a CRM/team-workspace tool. Primary conversion action: contact via mailto
link (no self-signup flow yet).

## Content structure

Single page, sections in order:

1. **Header/nav** — "T-Slen CRM" wordmark, sticky nav with anchor links
   (Features, Why T-Slen, Open Source, Contact), "Contact us" button.
2. **Hero** — headline + subheadline on the core value prop (all-in-one
   CRM/HR/tasks/chat for SMBs). Primary CTA: "Contact us" (mailto). Secondary
   link to the GitHub repo.
3. **Features grid** — cards for standout capabilities, sourced from the
   actual T-Slen CRM feature set (see `packages/tslen-crm` resources):
   Task & Project Management, HR & Scheduling, Team Chat, Video Meetings,
   Inventory, Analytics Dashboard (see `src/resources` and
   `packages/web/src/app` in the `tslen-crm` repo). Each card: inline SVG
   icon + one-line description.
4. **Why T-Slen** — 3 short value-prop columns: all-in-one (no tool
   sprawl), self-hosted (data stays yours), open source (MIT licensed).
5. **Open source callout** — short section noting the project is open
   source, tech-stack badges (NestJS, Angular, PostgreSQL), link to the
   GitHub repo (`github.com/T-slen-CRM/tslen-crm`).
6. **Contact/CTA band** — closing section with a mailto contact link.
7. **Footer** — copyright, links (GitHub, t-slen.com).

## Visual design

- **Palette**: dark base (`#0b0f14`-ish background, off-white text), single
  accent color (indigo/blue, ~`#4f7cff`) for CTAs/links/highlights. Defined
  as CSS custom properties on `:root` so a light variant can be swapped in
  via `prefers-color-scheme: light` without duplicating rules.
- **Typography**: system font stack (no web font download). Clear type
  scale: hero ~3.5rem down to body ~1rem. Generous line-height.
- **Layout**: max-width content container (~1200px), CSS Grid for the
  features section, flexbox for nav/hero. Mobile-first responsive.
- **Motion**: subtle hover states only; smooth-scroll for anchor nav; no
  heavy animation.
- **Icons**: hand-picked inline SVGs, no icon font/library dependency.

## Technical approach

Plain semantic HTML5 + one CSS file + minimal vanilla JS. No build step,
no framework, no external CDN dependency — deploys to GitHub Pages by
just enabling Pages on the branch root.

Files:
- `index.html` — semantic landmarks (`header`, `nav`, `main`, `section`,
  `footer`), proper heading hierarchy, `alt` text on any images, viewport
  meta, Open Graph + Twitter Card meta tags, `<title>` + meta description
  for SEO.
- `styles.css` — design tokens as custom properties, responsive layout,
  dark/light via `prefers-color-scheme`.
- `script.js` — mobile nav toggle, smooth-scroll for anchor links only.
- `favicon.svg`
- `og-image.png` — simple generated social-preview image.
- `CNAME` — contains `t-slen.com`, required for GitHub Pages custom domain.

## Deployment

- New repo `T-slen-CRM/t-slen.com`, **public**, created under the
  `T-slen-CRM` GitHub org.
- GitHub Pages enabled, serving from `main` branch, root.
- Custom domain set to `t-slen.com` via the `CNAME` file; DNS records
  (A/ALIAS or CNAME, per GitHub Pages docs) to be configured by the repo
  owner outside of this workflow.

## Testing

No automated tests (static marketing page). Manual verification before
push: responsive check at mobile/tablet/desktop widths, keyboard
navigation, WCAG AA color contrast, no console errors.
