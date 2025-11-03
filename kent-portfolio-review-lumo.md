# Kent Schaeffer – Portfolio Review
*Technical Writer · DDI Engineer · Web Developer*

---

## 1️⃣ ™ Is a single‑page app the right format?

| Pros of the current SPA | Cons / Risks |
|--------------------------|--------------|
| • All content reachable without a full page reload – great for a short, linear portfolio.<br>• Vue’s reactivity makes the dark‑mode toggle, scroll‑spy, and modal gallery easy to maintain.<br>• One URL simplifies sharing (https://kentknowsme.com). | • Search‑engine bots rely on JavaScript execution; if a crawler fails to run Vue the content may be invisible to SEO.<br>• Every asset (large images, the whole Vue bundle) loads on the first visit, even if a visitor only wants the “Contact” section.<br>• As the portfolio grows the page height will increase, causing longer scroll times and a larger “time‑to‑interactive”. |

**Recommendation**  
*Keep the SPA* if you value the smooth‑scroll experience and you don’t expect the site to become content‑heavy.  
If you anticipate adding a blog, case studies, or a downloadable résumé, consider **splitting the site into a few static pages** (`/about`, `/projects`, `/contact`) and using a lightweight router (Vue Router or plain anchor navigation). That lets browsers cache each page separately and improves SEO without losing the modern feel.

---

## 2️⃣ ™ Code‑base efficiency – Vue, HTML & CSS

### What works well

| Area | Observation |
|------|-------------|
| **Vue 3 (global build)** | `vue.global.prod.min.js` is a compact, production‑ready bundle. |
| **Bootstrap 5** | Provides a solid grid, responsive utilities, and the modal component you already use. |
| **CSS custom properties** | Switching to a `light-mode` class is a clean way to toggle themes. |
| **IntersectionObserver** | Lazy‑reveals timeline items, education cards, etc., saving paint work. |
| **Debounced scroll handling** | Only one `scroll` listener, updating a few booleans. |

### Where you can tighten things

| Issue | Why it matters | Quick fix |
|-------|----------------|-----------|
| **Inline `<style>` block** | Prevents caching → larger initial payload. | Move CSS to `assets/css/main.css` (or a minified version). |
| **Large background images** | Slower first paint on slow connections. | Use `srcset` with multiple widths and add `loading="lazy"` for non‑critical images. |
| **Bootstrap JS + Vue** | Both manipulate the DOM → extra kilobytes. | Replace the modal with a tiny Vue‑only component, or keep Bootstrap but strip unused plugins via a custom build. |
| **Undefined colour variable** (`var(--primary-color)`) | Slider background refers to a variable that doesn’t exist. | Add an alias: `:root { --primary-color: var(--primary-sand); }`. |
| **No lazy loading for portfolio/gallery images** | Users download images they never view. | Add `loading="lazy"` to every `<img>` inside modals and the timeline. |
| **Placeholder Formspree ID** | Submissions will fail. | Register a real Formspree (or Netlify Forms) endpoint and store the ID in a config variable. |
| **No Subresource Integrity (SRI)** | Potential supply‑chain attack vector. | Add `integrity` and `crossorigin="anonymous"` attributes for all CDN resources. |
| **Missing ARIA landmarks** | Screen readers may skip sections. | Add `role="region"` and `aria-labelledby="section-id-heading"` to each major `<section>`. |
| **Focus management for modals** | Keyboard users may lose context after a modal opens. | After opening a modal, programmatically move focus to the modal heading: `document.getElementById('galleryModalLabel').focus();`. |
| **No `prefers-reduced-motion` handling** | Animations may upset motion‑sensitive users. | Wrap transition rules in `@media (prefers-reduced-motion: reduce) { … }`. |
| **Hard‑coded Formspree endpoint in `submitForm()`** | If the endpoint changes, the form breaks silently. | Externalize the endpoint URL to a constant or environment variable and reference it in the fetch call. |
| **Direct DOM manipulation (`document.body.classList.toggle`) inside Vue methods** | Bypasses Vue’s reactivity system and can cause state drift. | Bind a class to the root element via Vue’s `:class` syntax (e.g., `<body :class="{ 'light-mode': !darkMode }">`). |
| **Repeated inline event handlers (`@click="scrollToSection('education')"` etc.)** | Minor duplication; harder to maintain if URLs change. | Create a reusable component `<ScrollLink target="education">Education</ScrollLink>` that emits the scroll event. |
| **No explicit `type="button"` on non‑form buttons** | Browsers may treat them as submit buttons, causing unintended form submissions. | Add `type="button"` to all `<button>` elements that are not meant to submit a form. |
| **Absence of a CSP (Content Security Policy)** | Allows any script/resource to run, increasing XSS risk. | Add a CSP header via your hosting platform (e.g., `Content-Security-Policy: default-src 'self'; script-src 'self' https://cdnjs.cloudflare.com; style-src 'self' https://cdnjs.cloudflare.com https://fonts.bunny.net; img-src 'self' data: https://images.unsplash.com;`). |
| **No error handling for the modal image loading** | Broken image URLs result in empty placeholders. | Add an `onerror` handler to replace failed images with a fallback: `<img ... @error="e => e.target.src='assets/img/fallback.png'">`. |
| **Hard‑coded colour values in inline styles (`style="color: var(--primary-color)"`)** | Breaks if the CSS variable changes or is removed. | Use CSS classes instead of inline styles, e.g., `<h4 class="text-primary">`. |
| **No unit tests or linting in the repo** | Bugs can slip in unnoticed, especially with many manual edits. | Add `eslint` with `eslint-plugin-vue` and a simple Jest/Vitest test suite for critical components. |
| **No fallback for unsupported browsers (e.g., IE11)** | Some corporate environments still use older browsers. | Provide a basic polyfill bundle or graceful degradation notice for unsupported browsers. |

## 3️⃣ ™ Color scheme – is it attractive?

| Current palette | Visual impression |
|-----------------|-------------------|
| **Primary sand** `#D4A574` – warm, earthy.<br>**Secondary sage** `#7A9B76` – muted green.<br>**Accent terracotta** `#C67B5C` – reddish‑orange.<br>**Midnight background** `#0f1923` – deep navy/black. | Warm, professional, slightly “heritage” vibe. Works for a technical writer who wants to feel grounded and trustworthy. |
| **Dark‑mode** – background stays midnight, text is white.<br>**Light‑mode** – background becomes `#f5f5f5` (light gray) with dark text. | Contrast is sufficient, but the sand/green combo can feel a bit dated for a modern dev portfolio. |

### Suggested tweaks (optional)

1. **Introduce a cooler accent** (e.g., teal `#2AB7CA` or electric blue `#0099FF`) for CTA buttons.  
2. **Make the dark background a true charcoal** (`#212121`) rather than near‑black navy.  
3. **Add a subtle gradient** to the hero overlay (`linear-gradient(rgba(0,0,0,.5), rgba(0,0,0,.2)`).  
4. **Use a soft off‑white** (`#FAFAFA`) for the light theme instead of pure `#f5f5f5`.  

If you keep the current palette, double‑check WCAG contrast ratios for all UI states (hover, focus, disabled). The **focus outlines** (`outline: 3px solid var(--secondary-sage)`) should be bright enough; consider using the accent terracotta for focus rings to guarantee > 3:1 contrast.

---

## 4️⃣ ™ Layout & content – logical and appealing?

| Section | Strengths | Opportunities |
|---------|-----------|----------------|
| **Cover / Hero** | Large, striking beach image; clear headline; CTA button. | Add a short sub‑headline that mentions your core value proposition (“20 years of DDI, scripting & technical writing”). |
| **Portfolio circle** | Creative “donut” arrangement showcases eight skill categories. | On small screens the circle collapses to a grid – fine. Add **ARIA labels** to each card (`aria-label="Open DDI Consulting details"`). |
| **Education list** | Uses the bookmark‑star SVG you asked for; each entry includes a link. | Group certifications under a collapsible accordion to reduce vertical length on mobile. |
| **Timeline / Projects** | Chronological, includes images, roles, employer links. | Add **micro‑data** (`schema.org/JobPosting` or `CreativeWork`) so search engines can surface your experience. |
| **Contact form** | Simple, uses Formspree, includes required fields. | Add **reCAPTCHA v3** (or a honeypot) to block spam. Also add `novalidate` + custom error messages for better accessibility. |
| **Footer** | Social icons, Unsplash attribution, copyright. | Include a **“Download résumé (PDF)”** link; a small dropdown that triggers a serverless function to generate PDF/Markdown/LibreOffice versions would be a nice touch. |
| **Navigation** | Fixed header, scroll‑spy highlighting active section. | The dark‑mode toggle sits inside the nav list; on narrow screens it may be hidden behind the collapsed menu. Ensure the toggle is **always visible** (move it outside the collapsible `<ul>`). |

Overall the flow is **logical**: Hero → Portfolio → Education → Projects → Contact.  
The only friction point is the length of the timeline; a “Show more / less” button after the first 4 items would keep the page scannable.

---

## 5️⃣ ™ Getting the dark‑mode / light‑mode slider to work

### Why it’s broken right now

| Problem | Fix |
|---------|-----|
| `darkMode` defaults to `true`, but the page loads with the dark palette (`--bg-midnight`). The `light-mode` class is never added on first render, so the toggle appears “on” but the UI stays dark. | Initialise `darkMode` based on a persisted setting or the OS preference: `darkMode: window.matchMedia('(prefers-color-scheme: dark)').matches` and then call `applyTheme()` in `mounted()`. |
| Sun/moon icons never change colour when the theme switches. | Add CSS rules that react to the body class, e.g. `body.light-mode .mode-icon { color: var(--text-dark); }`. |
| The toggle UI does not remember the state after a page reload. | Store the preference in `localStorage` and read it back in `mounted()`. |
| Slider background uses `var(--primary-color)` which is undefined. | Add an alias: `:root { --primary-color: var(--primary-sand); }`. |
| No transition for background colour when the class changes. | Add `transition: background-color .3s ease, color .3s ease;` to `body`. |

### Minimal working snippet

```js
// data()
return {
  darkMode: false,   // will be overwritten in mounted()
  // …other data
};

// mounted()
mounted() {
  const saved = localStorage.getItem('theme');
  if (saved) {
    this.darkMode = saved === 'dark';
  } else {
    // Respect OS preference
    this.darkMode = window.matchMedia('(prefers-color-scheme: dark)').matches;
  }
  this.applyTheme();          // sets the correct class
  // existing listeners …
},

methods: {
  toggleDarkMode() {
    this.darkMode = !this.darkMode;
    this.applyTheme();
  },
  applyTheme() {
    document.body.classList.toggle('light-mode', !this.darkMode);
    localStorage.setItem('theme', this.darkMode ? 'dark' : 'light');
  },
  // …other methods
}
```

## 6️⃣ ™ Checklist – actions to turn this site into a demand‑generator

### 🔥 High‑impact (do first)

| ✅ | Task | Why it matters |
|---|------|----------------|
| 1 | **Separate the CSS** – move the `<style>` block to `assets/css/main.css` and minify it. | Enables browser caching, reduces HTML size, improves first‑paint. |
| 2 | **Add Subresource Integrity (SRI)** to every CDN `<script>` / `<link>`. | Protects against supply‑chain tampering. |
| 3 | **Fix the dark‑mode toggle** – store the theme in `localStorage`, correct the missing `--primary-color` variable, and add a smooth transition on `body`. | Gives users reliable control and shows polish. |
| 4 | **Lazy‑load all images** – add `loading="lazy"` and a `srcset` with several widths for the hero, portfolio cards, timeline pictures, and gallery images. | Saves bandwidth, especially on mobile, and improves page‑load time. |
| 5 | **Add proper ARIA landmarks** (`role="region"` + `aria-labelledby="…"`) on each `<section>`. | Makes the site navigable for screen‑reader users. |
| 6 | **Replace the placeholder Formspree ID** with a real endpoint (or Netlify Forms) and add a hidden honeypot field. | Prevents broken submissions and reduces spam. |
| 7 | **Run Lighthouse** (Chrome DevTools → Audits) and fix any performance, accessibility, or best‑practice warnings it flags. | Gives you a measurable baseline and concrete improvement targets. |
| 8 | **Add a “Download résumé” button** (PDF, Markdown, LibreOffice) in the footer or a dedicated section. | Provides recruiters a ready‑to‑save artifact. |
| 9 | **Add micro‑data** (`schema.org/Person`, `JobPosting`, `CreativeWork`) to the HTML head and relevant sections. | Helps search engines surface your experience in rich results. |

### 📈 Medium‑impact (once the basics are solid)

| ✅ | Task | Why it matters |
|---|------|----------------|
|10| **Create a custom Bootstrap build** (only `modal` and `collapse`). | Reduces the JavaScript payload by ~30 KB. |
|11| **Migrate to Vite** (or Vue CLI) for bundling, hashing, and hot‑module reload. | Future‑proofs the project and enables code‑splitting. |
|12| **Add a “Show more / less”** toggle for the timeline after the first 4 items. | Keeps the page scannable while still showing full history on demand. |
|13| **Swap the hero beach photo** for a tech‑oriented image (abstract pattern, circuit board, etc.). | Aligns visual branding with your DDI / development expertise. |
|14| **Introduce a cooler accent colour** (e.g., teal `#2AB7CA` or electric blue `#0099FF`) for CTA buttons and focus rings. | Draws attention to key actions (Contact, View Portfolio). |
|15| **Add a print‑friendly stylesheet** (`@media print`) that hides navigation, removes backgrounds, and forces black‑on‑white text. | Gives recruiters a clean printable version. |
|16| **Add a sticky “Hire Me” banner** (bottom‑right) linking to a Calendly/Outlook booking page. | Low‑friction way for prospects to schedule a call. |
|17| **Install a lightweight analytics snippet** (Plausible, Umami, or GoatCounter). | Shows which sections attract the most interest, guiding future tweaks. |

### 🧹 Low‑impact / polish (nice‑to‑have)

| ✅ | Action | How to implement | Benefit |
|---|--------|------------------|---------|
|18| **Respect reduced‑motion preferences** | `@media (prefers-reduced-motion: reduce) { .portfolio-item, .timeline-item, .education-item, .fade-in { transition:none; } }` | Prevents motion‑sensitive users from being startled. |
|19| **Add a favicon & Apple touch icon** | Place `favicon.png`/`favicon.svg` in `assets/img/` and add `<link rel="icon" href="assets/img/favicon.png">` and `<link rel="apple-touch-icon" href="assets/img/favicon.png">`. | Professional polish in browser tabs and on mobile home screens. |
|20| **Add a “Last Updated” meta tag** | `<meta name="last-modified" content="2025-11-01">` (update whenever you push a new version). | Signals freshness to both users and search engines. |
|21| **Create `robots.txt`** (`User-agent: *\nAllow: /`). | Explicitly tells crawlers what they may index. |
|22| **Generate a simple `sitemap.xml`** listing the main anchors (`/`, `/#portfolio`, `/#education`, `/#projects`, `/#contact`). | Helps search engines discover each section reliably. |
|23| **Add subtle hover‑scale to social icons** | Extend `.social-links a:hover { transform: scale(1.1); transition: transform .2s; }`. | Improves visual feedback and makes the icons feel interactive. |
|24| **Keep the “Skip to main content” link** functional and styled (`:focus { top:0; }`). | Enhances keyboard navigation for assistive‑technology users. |
|25| **Optional: Add an “About Me” modal** | Small hidden modal triggered by the logo or an “About” link; contains a 2‑paragraph bio + photo. | Gives visitors a quick personal snapshot without leaving the page. |

---

### How to use this checklist

1. **Start with the high‑impact items (1‑9).** They give the biggest ROI in performance, security, and usability.  
2. **Move on to medium‑impact tasks (10‑17)** once the fundamentals are solid.  
3. **Finish with the low‑impact polish (18‑25)** to add the finishing touches that impress detail‑oriented reviewers.

Tackle them in order, test after each batch (run Lighthouse again, check accessibility with a screen‑reader, verify the dark‑mode toggle, and confirm that all links work), and you’ll have a fast, secure, and highly professional portfolio that showcases your 20 + years of technical writing, DDI, and web‑development expertise.

Happy building, Kent! 🚀