# T-Slen CRM Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a single-page static marketing site for T-Slen CRM, live at `https://t-slen.com` via GitHub Pages.

**Architecture:** Plain semantic HTML5 (`index.html`) + one CSS file (`styles.css`, custom-property design tokens, mobile-first, `prefers-color-scheme` light variant) + one small vanilla JS file (`script.js`, mobile nav toggle only). No build step, no framework, no external dependency. Deployed by pushing to a new public repo and enabling GitHub Pages with a `CNAME` file.

**Tech Stack:** HTML5, CSS3 (custom properties, Grid, Flexbox), vanilla JS (ES2017+), GitHub Pages.

**Spec:** `/Users/olegteslenko/Desktop/t-slen.com/docs/superpowers/specs/2026-08-16-landing-page-design.md`

## Global Constraints

- No build step, no framework, no CDN dependency (spec: "Technical approach").
- Mobile-first, responsive at mobile/tablet/desktop widths (spec: "Testing").
- WCAG AA color contrast (spec: "Testing").
- Dark base palette with a light variant via `prefers-color-scheme` sharing the same CSS custom-property tokens (spec: "Visual design").
- System font stack only — no web font downloads (spec: "Visual design").
- Primary CTA is a `mailto:` contact link, not a signup flow (spec: "Purpose").
- All copy about features must reflect the real T-Slen CRM feature set (Task & Project Management, HR & Scheduling, Team Chat, Video Meetings, Inventory, Analytics Dashboard) — not invented features.
- Repo: `T-slen-CRM/t-slen.com`, public, GitHub Pages serving `main` branch root, custom domain `t-slen.com` (spec: "Deployment").

---

## File Structure

- `index.html` — all page markup. Built incrementally: Task 1 creates the shell with HTML comment anchors (`<!-- HERO -->`, `<!-- FEATURES -->`, etc.) inside `<main>`; each later task replaces its anchor comment with real markup.
- `styles.css` — single stylesheet, appended to task by task (tokens/reset/nav in Task 1, then one block per section).
- `script.js` — mobile nav toggle behavior (Task 6).
- `favicon.svg`, `og-image.png` — static assets (Task 7).
- `CNAME` — GitHub Pages custom domain file, contains `t-slen.com` (Task 7).
- `docs/superpowers/specs/2026-08-16-landing-page-design.md` — already exists (the spec).

## Task 1: Page shell — head, header/nav, footer, CSS tokens & reset

**Files:**
- Create: `index.html`
- Create: `styles.css`

**Interfaces:**
- Produces: HTML anchor comments in `index.html` inside `<main id="top">`: `<!-- HERO -->`, `<!-- FEATURES -->`, `<!-- WHY -->`, `<!-- OPEN_SOURCE -->`, `<!-- CONTACT -->` — later tasks replace these one at a time.
- Produces: CSS custom properties on `:root` (`--color-bg`, `--color-bg-alt`, `--color-text`, `--color-text-muted`, `--color-accent`, `--color-accent-hover`, `--color-border`, `--radius`, `--container-width`, `--font-sans`) and a `prefers-color-scheme: light` override block — later tasks' CSS uses these variables.
- Produces: `.container`, `.btn`, `.btn-primary`, `.btn-ghost`, `.btn-lg` utility classes — used by every later section.
- Produces: `#navToggle` button and `#primaryNav` nav element — consumed by Task 6's JS.

- [ ] **Step 1: Write `index.html`**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>T-Slen CRM — All-in-one CRM & team workspace for growing businesses</title>
  <meta name="description" content="T-Slen CRM combines CRM, HR, task management, scheduling, video meetings and team chat in one self-hosted, open-source platform for small and medium businesses.">
  <meta property="og:title" content="T-Slen CRM — All-in-one CRM & team workspace">
  <meta property="og:description" content="CRM, HR, tasks, scheduling, video meetings and team chat — one open-source, self-hosted platform for SMBs.">
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://t-slen.com">
  <meta property="og:image" content="https://t-slen.com/og-image.png">
  <meta name="twitter:card" content="summary_large_image">
  <link rel="icon" type="image/svg+xml" href="favicon.svg">
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container header-inner">
      <a class="logo" href="#top">T-Slen<span class="accent">CRM</span></a>
      <button class="nav-toggle" id="navToggle" type="button" aria-expanded="false" aria-controls="primaryNav" aria-label="Toggle navigation">
        <span></span><span></span><span></span>
      </button>
      <nav class="primary-nav" id="primaryNav">
        <a href="#features">Features</a>
        <a href="#why">Why T-Slen</a>
        <a href="#open-source">Open Source</a>
        <a href="#contact" class="btn btn-primary">Contact us</a>
      </nav>
    </div>
  </header>

  <main id="top">
    <!-- HERO -->
    <!-- FEATURES -->
    <!-- WHY -->
    <!-- OPEN_SOURCE -->
    <!-- CONTACT -->
  </main>

  <footer class="site-footer">
    <div class="container footer-inner">
      <span>&copy; 2026 T-Slen CRM. MIT Licensed.</span>
      <div class="footer-links">
        <a href="https://github.com/T-slen-CRM/tslen-crm" target="_blank" rel="noopener">GitHub</a>
        <a href="https://t-slen.com">t-slen.com</a>
      </div>
    </div>
  </footer>

  <script src="script.js"></script>
</body>
</html>
```

- [ ] **Step 2: Write `styles.css`**

```css
:root {
  --color-bg: #0b0f14;
  --color-bg-alt: #12171f;
  --color-text: #e6ebf1;
  --color-text-muted: #9aa5b1;
  --color-accent: #4f7cff;
  --color-accent-hover: #6f93ff;
  --color-border: #232a35;
  --radius: 10px;
  --container-width: 1200px;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

@media (prefers-color-scheme: light) {
  :root {
    --color-bg: #ffffff;
    --color-bg-alt: #f5f7fa;
    --color-text: #12171f;
    --color-text-muted: #52606d;
    --color-accent: #3660e0;
    --color-accent-hover: #2748b8;
    --color-border: #e2e8f0;
  }
}

* { box-sizing: border-box; }
html { scroll-behavior: smooth; }
body {
  margin: 0;
  font-family: var(--font-sans);
  background: var(--color-bg);
  color: var(--color-text);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}
img, svg { max-width: 100%; display: block; }
a { color: inherit; text-decoration: none; }
h1, h2, h3, p { margin: 0; }
.container { max-width: var(--container-width); margin: 0 auto; padding: 0 1.5rem; }

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.65rem 1.25rem;
  border-radius: var(--radius);
  font-weight: 600;
  font-size: 0.95rem;
  border: 1px solid transparent;
  cursor: pointer;
  transition: background-color 0.15s ease, border-color 0.15s ease, color 0.15s ease;
}
.btn-primary { background: var(--color-accent); color: #fff; }
.btn-primary:hover { background: var(--color-accent-hover); }
.btn-ghost { border-color: var(--color-border); color: var(--color-text); }
.btn-ghost:hover { border-color: var(--color-accent); color: var(--color-accent); }
.btn-lg { padding: 0.85rem 1.75rem; font-size: 1.05rem; }

.site-header {
  position: sticky;
  top: 0;
  z-index: 50;
  background: rgba(11, 15, 20, 0.85);
  backdrop-filter: blur(8px);
  border-bottom: 1px solid var(--color-border);
}
@media (prefers-color-scheme: light) {
  .site-header { background: rgba(255, 255, 255, 0.85); }
}
.header-inner { display: flex; align-items: center; justify-content: space-between; height: 64px; }
.logo { font-weight: 700; font-size: 1.15rem; }
.logo .accent { color: var(--color-accent); }
.primary-nav { display: flex; align-items: center; gap: 1.75rem; }
.primary-nav a:not(.btn) { color: var(--color-text-muted); font-weight: 500; font-size: 0.95rem; }
.primary-nav a:not(.btn):hover { color: var(--color-text); }
.nav-toggle { display: none; flex-direction: column; gap: 4px; background: none; border: none; cursor: pointer; padding: 0.5rem; }
.nav-toggle span { width: 22px; height: 2px; background: var(--color-text); display: block; }

.site-footer { border-top: 1px solid var(--color-border); padding: 2rem 0; }
.footer-inner { display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 1rem; color: var(--color-text-muted); font-size: 0.9rem; }
.footer-links { display: flex; gap: 1.25rem; }
.footer-links a:hover { color: var(--color-text); }
```

- [ ] **Step 3: Verify the shell renders**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
python3 -m http.server 8000 &>/tmp/tslen-http.log &
sleep 1
curl -s http://localhost:8000/ | grep -c '<!-- HERO -->\|<!-- FEATURES -->\|<!-- WHY -->\|<!-- OPEN_SOURCE -->\|<!-- CONTACT -->'
kill %1
```
Expected: `5` (all five anchor comments present).

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: add page shell, nav, footer, and design tokens"
```

## Task 2: Hero section

**Files:**
- Modify: `index.html` (replace `<!-- HERO -->`)
- Modify: `styles.css` (append)

**Interfaces:**
- Consumes: `<!-- HERO -->` anchor from Task 1, `.container`, `.btn`, `.btn-primary`, `.btn-ghost`, `.btn-lg` from Task 1.
- Produces: `.hero`, `.hero-inner`, `.hero-sub`, `.hero-actions` classes.

- [ ] **Step 1: Replace the `<!-- HERO -->` comment in `index.html`**

```html
    <section class="hero">
      <div class="container hero-inner">
        <h1>Run your whole business from one workspace</h1>
        <p class="hero-sub">T-Slen CRM brings CRM, HR, tasks, scheduling, video meetings and team chat together — self-hosted, open source, and built for small and medium businesses.</p>
        <div class="hero-actions">
          <a href="#contact" class="btn btn-primary btn-lg">Contact us</a>
          <a href="https://github.com/T-slen-CRM/tslen-crm" class="btn btn-ghost btn-lg" target="_blank" rel="noopener">View on GitHub</a>
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Append to `styles.css`**

```css
.hero { padding: 6rem 0 5rem; }
.hero-inner { max-width: 780px; }
.hero h1 { font-size: clamp(2.25rem, 5vw, 3.5rem); line-height: 1.15; margin: 0 0 1.25rem; }
.hero-sub { font-size: 1.15rem; color: var(--color-text-muted); margin: 0 0 2rem; max-width: 620px; }
.hero-actions { display: flex; gap: 1rem; flex-wrap: wrap; }
```

- [ ] **Step 3: Verify**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
grep -c 'class="hero"' index.html
grep -c 'Run your whole business' index.html
```
Expected: both `1`.

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: add hero section"
```

## Task 3: Features grid

**Files:**
- Modify: `index.html` (replace `<!-- FEATURES -->`)
- Modify: `styles.css` (append)

**Interfaces:**
- Consumes: `<!-- FEATURES -->` anchor from Task 1, `.container` from Task 1.
- Produces: `.features`, `.feature-grid`, `.feature-card`, `.feature-icon` classes.

- [ ] **Step 1: Replace the `<!-- FEATURES -->` comment in `index.html`**

```html
    <section id="features" class="features">
      <div class="container">
        <h2>Everything your team needs, in one place</h2>
        <div class="feature-grid">
          <article class="feature-card">
            <svg class="feature-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 11l3 3L22 4"/><path d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"/></svg>
            <h3>Task &amp; Project Management</h3>
            <p>Plan work into projects and phases, and track every task from idea to done.</p>
          </article>
          <article class="feature-card">
            <svg class="feature-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg>
            <h3>HR &amp; Scheduling</h3>
            <p>Manage job positions, days-off approvals, and personal or team calendars, synced with Google Calendar.</p>
          </article>
          <article class="feature-card">
            <svg class="feature-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>
            <h3>Team Chat</h3>
            <p>Real-time messaging keeps conversations and decisions where the work happens.</p>
          </article>
          <article class="feature-card">
            <svg class="feature-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M23 7l-7 5 7 5V7z"/><rect x="1" y="5" width="15" height="14" rx="2"/></svg>
            <h3>Video Meetings</h3>
            <p>Jump into built-in video calls, with Google Meet integration for scheduled meetings.</p>
          </article>
          <article class="feature-card">
            <svg class="feature-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 8V21H3V8"/><path d="M1 3h22v5H1z"/><path d="M10 12h4"/></svg>
            <h3>Inventory</h3>
            <p>Keep track of company assets and stock without leaving your workspace.</p>
          </article>
          <article class="feature-card">
            <svg class="feature-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 20V10M12 20V4M6 20v-6"/></svg>
            <h3>Analytics Dashboard</h3>
            <p>See how your team and company are performing at a glance.</p>
          </article>
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Append to `styles.css`**

```css
.features { padding: 5rem 0; background: var(--color-bg-alt); border-top: 1px solid var(--color-border); border-bottom: 1px solid var(--color-border); }
.features h2 { font-size: 2rem; margin: 0 0 2.5rem; text-align: center; }
.feature-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.75rem; }
.feature-card { background: var(--color-bg); border: 1px solid var(--color-border); border-radius: var(--radius); padding: 1.75rem; }
.feature-icon { width: 32px; height: 32px; color: var(--color-accent); margin-bottom: 1rem; }
.feature-card h3 { margin: 0 0 0.5rem; font-size: 1.05rem; }
.feature-card p { margin: 0; color: var(--color-text-muted); font-size: 0.95rem; }
@media (max-width: 860px) { .feature-grid { grid-template-columns: repeat(2, 1fr); } }
@media (max-width: 560px) { .feature-grid { grid-template-columns: 1fr; } }
```

- [ ] **Step 3: Verify**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
grep -c 'class="feature-card"' index.html
```
Expected: `6`.

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: add features grid section"
```

## Task 4: Why T-Slen & Open Source sections

**Files:**
- Modify: `index.html` (replace `<!-- WHY -->` and `<!-- OPEN_SOURCE -->`)
- Modify: `styles.css` (append)

**Interfaces:**
- Consumes: `<!-- WHY -->`, `<!-- OPEN_SOURCE -->` anchors from Task 1, `.container`, `.btn-ghost` from Task 1.
- Produces: `.why`, `.why-grid`, `.why-item`, `.open-source`, `.open-source-inner`, `.tech-badges`, `.badge` classes.

- [ ] **Step 1: Replace the `<!-- WHY -->` comment in `index.html`**

```html
    <section id="why" class="why">
      <div class="container">
        <h2>Why teams choose T-Slen</h2>
        <div class="why-grid">
          <div class="why-item">
            <h3>All-in-one</h3>
            <p>Replace a stack of disconnected tools with a single workspace for CRM, HR, tasks, and communication.</p>
          </div>
          <div class="why-item">
            <h3>Self-hosted</h3>
            <p>Deploy it yourself and keep full ownership of your company's data.</p>
          </div>
          <div class="why-item">
            <h3>Open source</h3>
            <p>MIT licensed and open for anyone to inspect, extend, or contribute to.</p>
          </div>
        </div>
      </div>
    </section>
```

- [ ] **Step 2: Replace the `<!-- OPEN_SOURCE -->` comment in `index.html`**

```html
    <section id="open-source" class="open-source">
      <div class="container open-source-inner">
        <h2>Built in the open</h2>
        <p>T-Slen CRM is developed as an open-source project, built with NestJS, Angular, and PostgreSQL. Explore the code, self-host it, or contribute.</p>
        <div class="tech-badges">
          <span class="badge">NestJS</span>
          <span class="badge">Angular</span>
          <span class="badge">PostgreSQL</span>
          <span class="badge">MIT License</span>
        </div>
        <a href="https://github.com/T-slen-CRM/tslen-crm" class="btn btn-ghost" target="_blank" rel="noopener">View source on GitHub</a>
      </div>
    </section>
```

- [ ] **Step 3: Append to `styles.css`**

```css
.why { padding: 5rem 0; }
.why h2 { font-size: 2rem; margin: 0 0 2.5rem; text-align: center; }
.why-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 2rem; }
.why-item h3 { margin: 0 0 0.5rem; }
.why-item p { margin: 0; color: var(--color-text-muted); }
@media (max-width: 720px) { .why-grid { grid-template-columns: 1fr; gap: 2.5rem; } }

.open-source { padding: 5rem 0; background: var(--color-bg-alt); border-top: 1px solid var(--color-border); border-bottom: 1px solid var(--color-border); }
.open-source-inner { max-width: 640px; }
.open-source h2 { font-size: 2rem; margin: 0 0 1rem; }
.open-source p { color: var(--color-text-muted); margin: 0 0 1.5rem; }
.tech-badges { display: flex; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 1.75rem; }
.badge { font-size: 0.8rem; font-weight: 600; padding: 0.35rem 0.75rem; border-radius: 999px; border: 1px solid var(--color-border); color: var(--color-text-muted); }
```

- [ ] **Step 4: Verify**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
grep -c 'class="why-item"' index.html
grep -c 'class="badge"' index.html
```
Expected: `3` and `4`.

- [ ] **Step 5: Commit**

```bash
git add index.html styles.css
git commit -m "feat: add why-t-slen and open-source sections"
```

## Task 5: Contact band + footer styling

**Files:**
- Modify: `index.html` (replace `<!-- CONTACT -->`)
- Modify: `styles.css` (append)

**Interfaces:**
- Consumes: `<!-- CONTACT -->` anchor from Task 1, `.container`, `.btn-primary`, `.btn-lg`, `.site-footer`, `.footer-inner`, `.footer-links` markup from Task 1.
- Produces: `.contact-band` class.

- [ ] **Step 1: Replace the `<!-- CONTACT -->` comment in `index.html`**

```html
    <section id="contact" class="contact-band">
      <div class="container">
        <h2>Want to see it in action?</h2>
        <p>Get in touch and we'll help you get set up.</p>
        <a href="mailto:support@t-slen.com" class="btn btn-primary btn-lg">Email support@t-slen.com</a>
      </div>
    </section>
```

- [ ] **Step 2: Append to `styles.css`**

```css
.contact-band { padding: 5rem 0; text-align: center; }
.contact-band h2 { font-size: 2rem; margin: 0 0 0.75rem; }
.contact-band p { color: var(--color-text-muted); margin: 0 0 2rem; }
```

- [ ] **Step 3: Verify**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
grep -c 'mailto:support@t-slen.com' index.html
```
Expected: `1`.

- [ ] **Step 4: Commit**

```bash
git add index.html styles.css
git commit -m "feat: add contact band section"
```

## Task 6: Mobile nav toggle (JS + responsive CSS)

**Files:**
- Create: `script.js`
- Modify: `styles.css` (append)

**Interfaces:**
- Consumes: `#navToggle`, `#primaryNav` elements from Task 1.
- Produces: `.primary-nav.open` class toggled at runtime.

- [ ] **Step 1: Write `script.js`**

```javascript
document.addEventListener('DOMContentLoaded', () => {
  const toggle = document.getElementById('navToggle');
  const nav = document.getElementById('primaryNav');

  toggle.addEventListener('click', () => {
    const isOpen = nav.classList.toggle('open');
    toggle.setAttribute('aria-expanded', String(isOpen));
  });

  nav.querySelectorAll('a').forEach((link) => {
    link.addEventListener('click', () => {
      nav.classList.remove('open');
      toggle.setAttribute('aria-expanded', 'false');
    });
  });
});
```

- [ ] **Step 2: Append responsive nav CSS to `styles.css`**

```css
@media (max-width: 780px) {
  .nav-toggle { display: flex; }
  .primary-nav {
    position: absolute;
    top: 64px;
    left: 0;
    right: 0;
    background: var(--color-bg);
    border-bottom: 1px solid var(--color-border);
    flex-direction: column;
    align-items: flex-start;
    padding: 1.5rem;
    gap: 1.25rem;
    display: none;
  }
  .primary-nav.open { display: flex; }
}
```

- [ ] **Step 3: Verify**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
grep -c "getElementById('navToggle')" script.js
```
Expected: `1`. Then manually confirm interactivity by serving the page and clicking the toggle at a narrow viewport:
```bash
python3 -m http.server 8000 &>/tmp/tslen-http.log &
sleep 1
open http://localhost:8000/
```
Resize the browser window to under 780px wide, click the hamburger icon, and confirm the nav menu opens and closes, then closes again when a nav link is clicked. Stop the server afterward: `kill %1`.

- [ ] **Step 4: Commit**

```bash
git add script.js styles.css
git commit -m "feat: add mobile nav toggle"
```

## Task 7: Favicon, OG image, and CNAME

**Files:**
- Create: `favicon.svg`
- Create: `og-image.png`
- Create: `CNAME`

**Interfaces:**
- Consumes: `<link rel="icon" ...>` and `<meta property="og:image" ...>` tags already in `index.html` from Task 1 (both already reference `favicon.svg` and `og-image.png` by filename — no HTML changes needed here).
- Produces: the three asset files referenced by Task 1's `<head>`.

- [ ] **Step 1: Write `favicon.svg`**

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="8" fill="#4f7cff"/>
  <text x="16" y="22" font-family="Arial, sans-serif" font-size="16" font-weight="700" fill="#ffffff" text-anchor="middle">T</text>
</svg>
```

- [ ] **Step 2: Generate `og-image.png`**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
python3 -c "import PIL" 2>/dev/null || pip3 install --quiet Pillow
python3 - <<'EOF'
from PIL import Image, ImageDraw, ImageFont

img = Image.new("RGB", (1200, 630), color=(11, 15, 20))
draw = ImageDraw.Draw(img)
draw.ellipse([-150, -150, 350, 350], fill=(18, 23, 31))

try:
    font_big = ImageFont.truetype("/System/Library/Fonts/Helvetica.ttc", 72)
    font_small = ImageFont.truetype("/System/Library/Fonts/Helvetica.ttc", 32)
except OSError:
    font_big = ImageFont.load_default()
    font_small = ImageFont.load_default()

draw.text((80, 250), "T-Slen CRM", font=font_big, fill=(230, 235, 241))
draw.text((80, 340), "All-in-one CRM & team workspace for SMBs", font=font_small, fill=(154, 165, 177))
img.save("og-image.png")
EOF
```

- [ ] **Step 3: Write `CNAME`**

```
t-slen.com
```

- [ ] **Step 4: Verify all three files exist and are non-empty**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
ls -la favicon.svg og-image.png CNAME
file og-image.png
```
Expected: all three listed with non-zero size; `file og-image.png` reports `PNG image data, 1200 x 630`.

- [ ] **Step 5: Commit**

```bash
git add favicon.svg og-image.png CNAME
git commit -m "feat: add favicon, og-image, and CNAME"
```

## Task 8: Full-page QA pass

**Files:**
- Modify: `index.html` or `styles.css` only if a check below fails (no changes expected if prior tasks were followed exactly).

**Interfaces:**
- Consumes: the complete `index.html` / `styles.css` / `script.js` from Tasks 1–7.

- [ ] **Step 1: Serve the full site locally and confirm every asset returns 200**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
python3 -m http.server 8000 &>/tmp/tslen-http.log &
sleep 1
for f in / styles.css script.js favicon.svg og-image.png; do
  code=$(curl -s -o /dev/null -w "%{http_code}" "http://localhost:8000/$f")
  echo "$f -> $code"
done
```
Expected: every line ends `-> 200`.

- [ ] **Step 2: Confirm all five sections and both nav states are present**

Run:
```bash
grep -c '<section' index.html
```
Expected: `5` (hero, features, why, open-source, contact).

- [ ] **Step 3: Manual visual + accessibility check**

With the server still running from Step 1, open `http://localhost:8000/` in a browser:
- Resize from mobile (~375px) to desktop (~1440px) width; confirm no horizontal scrollbar appears and the nav collapses to the hamburger below 780px.
- Tab through the page with the keyboard; confirm every link and button receives a visible focus outline and the tab order is logical (logo → nav links → Contact button → hero buttons → feature cards' links if any → footer links).
- Toggle macOS system appearance between light and dark (System Settings → Appearance) and confirm the page palette switches automatically without reloading.
- Confirm accent text (`--color-accent`) against both backgrounds passes WCAG AA (use the browser's accessibility inspector or https://webaim.org/resources/contrastchecker/ with `#4f7cff` on `#0b0f14`, and `#3660e0` on `#ffffff`).

Stop the server: `kill %1`.

- [ ] **Step 4: Check the browser console is clean**

With the dev tools console open while browsing the served page, confirm there are zero errors or warnings logged.

- [ ] **Step 5: Commit any fixes found during QA (skip if none needed)**

```bash
git add -A
git commit -m "fix: address QA findings from full-page pass"
```

## Task 9: Create the GitHub repo, push, and enable Pages

**Files:** none (repo/infra operations only).

**Interfaces:**
- Consumes: the finished local repo at `/Users/olegteslenko/Desktop/t-slen.com` (all prior tasks).

- [ ] **Step 1 (manual — user action): Create the empty repo**

Go to `github.com/organizations/T-slen-CRM/repositories/new`, set:
- Repository name: `t-slen.com`
- Visibility: **Public**
- Do **not** initialize with a README, `.gitignore`, or license (the local repo already has content and an initial README-less history; adding one on GitHub would create a conflicting commit).

Click "Create repository."

- [ ] **Step 2: Add the remote and push**

Run:
```bash
cd /Users/olegteslenko/Desktop/t-slen.com
git remote add origin git@github-tslen:T-slen-CRM/t-slen.com.git
git branch -M main
git push -u origin main
```
Expected: push succeeds and reports the new branch tracking `origin/main`.

- [ ] **Step 3 (manual — user action): Enable GitHub Pages**

Go to `github.com/T-slen-CRM/t-slen.com/settings/pages`:
- Under "Build and deployment" → "Source", select **Deploy from a branch**.
- Branch: `main`, folder: `/ (root)`. Save.
- Under "Custom domain", enter `t-slen.com` and save (this reads the `CNAME` file already in the repo; GitHub will show a DNS check warning until DNS is configured — that's expected at this point).

- [ ] **Step 4 (manual — user action): Configure DNS**

At your domain registrar/DNS provider for `t-slen.com`, add the four GitHub Pages A records for the apex domain, pointing to:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
If you also want `www.t-slen.com` to work, add a `CNAME` record for `www` pointing to `t-slen-crm.github.io`.

- [ ] **Step 5: Verify the deployment**

Run:
```bash
curl -s -o /dev/null -w "%{http_code}\n" https://t-slen-crm.github.io/t-slen.com/
```
Expected: `200` (confirms the Pages build succeeded on the `github.io` URL, independent of DNS propagation for the custom domain, which can take up to 24 hours).

Once DNS propagates, also run:
```bash
curl -s -o /dev/null -w "%{http_code}\n" https://t-slen.com/
```
Expected: `200`.
