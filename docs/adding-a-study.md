# Adding a New Study to DataTruth

This guide walks you through creating a new study page from scratch. Follow it top to bottom — the checklist at the end summarises every step.

---

## 1. Define the Study

Before writing any code, answer these three questions:

1. **What accountability question does this study answer?**
   - Good: "How much do state governments spend on public employee pensions, and which states are at risk of fiscal distress?"
   - Too vague: "How is money spent in India?"
   - Not accountability: "How has India's GDP grown?"

2. **What dataset answers it?**
   - Must be from a Government of India publication, RBI, CAG, or a peer-reviewed research institution
   - Must be publicly accessible (include the URL)
   - Must have enough granularity to be visualisable (state-wise, year-wise, or MP-wise)

3. **What is the single most important finding?**
   - This becomes the `stat-card` lead figure and the `study-finding` on the homepage card
   - It should be expressible in one sentence: "X% of Y is Z"

---

## 2. Choose a Study Number and Filename

Studies are numbered sequentially. The current highest is 06 (`pension-bomb.html`). Use the next available number.

Filename convention: `kebab-case-short-title.html`

Examples: `march-rush.html`, `state-debt.html`, `pension-bomb.html`

---

## 3. Copy the Template Structure

Start with the skeleton below. Replace all `[PLACEHOLDER]` values.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>[Study Title] — DataTruth</title>
  <meta name="description" content="[1–2 sentence plain-language description of what the study reveals]" />
  <meta property="og:title" content="[Study Title] — DataTruth" />
  <meta property="og:description" content="[Key finding in one sentence]" />
  <meta property="og:type" content="article" />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Playfair+Display:wght@700&display=swap" rel="stylesheet" />
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.3/dist/chart.umd.min.js"></script>
  <style>
    /* Copy the full CSS block from any existing study page.
       The design system is identical across all studies. */
  </style>
</head>
<body>
<div id="progress" style="position:fixed;top:0;left:0;height:3px;background:#dc2626;z-index:200;width:0%;pointer-events:none;transition:width 80ms linear"></div>

<nav>
  <a href="index.html" class="logo">Data<span>Truth</span></a>
  <ul class="nav-links">
    <li><a href="march-rush.html">The March Rush</a></li>
    <li><a href="mplads.html">MP's ₹5 Crore</a></li>
    <li><a href="absent-mp.html">The Absent MP</a></li>
    <li><a href="state-debt.html">State Debt</a></li>
    <li><a href="scheme-maze.html">Scheme Maze</a></li>
    <li><a href="pension-bomb.html">Pension Bomb</a></li>
    <li><a href="[your-file].html" class="active">[Short Nav Title]</a></li>
    <li><a href="credits.html">Credits</a></li>
  </ul>
</nav>

<div class="notice">
  📊 Showing indicative data based on [primary source] · [year range]
</div>

<section class="study-hero">
  <div class="container">
    <div class="breadcrumb"><a href="index.html">DataTruth</a> › Study [N]</div>
    <div class="study-tag">Study [N] · [Category]</div>
    <h1>[Study Title]</h1>
    <p class="study-hero-desc">
      [2–3 sentence description. What is the problem? What does the data show? Why does it matter to a citizen?]
    </p>
    <div class="analogy-pill">
      <strong>Analogy:</strong> [One-sentence everyday analogy that makes the fiscal concept concrete]
    </div>
  </div>
</section>

<!-- Continue with: summary-box, stat-grid, chart-cards, insight, incidents, ds-box, more-section, footer -->
<!-- See docs/architecture.md for full page anatomy -->
```

---

## 4. Write the Data

Define your data as JavaScript constants at the top of the `<script>` block. Use descriptive names.

```js
// Good
const STATE_PENSION_BURDEN = [
  { state: 'Punjab', pct: 24.0, color: '#dc2626' },
  { state: 'Himachal Pradesh', pct: 18.2, color: '#dc2626' },
  // ...
];

// Bad — anonymous arrays without context
const d = [[24.0, '#dc2626'], [18.2, '#dc2626']];
```

If you are modelling or scaling data, define the methodology inline:

```js
// NPS projection: 10% employee + 14% employer contribution, 8% p.a. net return
// OPS projection: no corpus; pay-as-you-go from revenue, 6% salary growth p.a.
const OPS_TRAJECTORY = [...];
const NPS_TRAJECTORY = [...];
```

---

## 5. Charts — Minimum Requirements

Every study page must have **at least 2 charts**. Each chart must have:

- A `.chart-title` (what is being shown)
- A `.chart-sub` (data source, year, unit)
- A `.chart-hint` with "How to read this:" guidance in plain language
- `role="img"` and `aria-label` on the `<canvas>` element
- A legend if there are multiple data series

Reference lines (targets, benchmarks, averages) must be labelled inline on the chart, not just in a legend.

---

## 6. Incident Cards

Every study needs **4–6 incident cards** documenting real-world consequences of the pattern being analysed.

Each card needs:
- A documented, specific incident (not a generic statement)
- A date or year
- A named source (CAG report number, newspaper with date, parliamentary committee report)

Good incident card:
> **Bihar bridges, June 2023** — 13 bridges collapsed in 18 days. The ₹1,710 crore Aguwani–Sultanganj bridge had failed twice. Engineers pointed to year-end budget pressure forcing DPRs through without quality scrutiny.
> *Source: CNN (June 6, 2023); National Herald India*

Bad incident card:
> **Infrastructure failures** — Many infrastructure projects fail due to poor quality sanctioning caused by rushed year-end spending.

---

## 7. Data Sources Box

The `ds-box` at the bottom of the page must list **every source used in the study** with:
- Full institutional name
- Direct URL
- 1–2 sentence description of what data was drawn from it
- The `method-box` must explain how indicative/modelled values were derived

---

## 8. Update Other Pages

When your study is ready, update:

1. **`index.html`** — add a new `study-card` in the studies grid
2. **All other study pages** — add your study to their `.more-section` grid (remove the page's own study from its own more-section)
3. **All nav bars** — add your study link to the `<nav>` on every page
4. **All footers** — add your study to the `footer-links` list on every page
5. **`credits.html`** — add your study to the "Datasets Used, Study by Study" section

---

## 9. Final Checklist

- [ ] Filename is `kebab-case.html`, study number is in sequence
- [ ] Meta description and OG tags are filled in
- [ ] Progress bar div added immediately after `<body>`
- [ ] Back-to-top button and JS added before `</body>`
- [ ] Notice banner accurately describes the data source and year range
- [ ] Hero has study-tag, h1, description paragraph, and analogy-pill
- [ ] Summary box has 3–5 specific findings
- [ ] 5–6 stat cards with labels, values, and sub-text
- [ ] At least 2 charts, each with title, sub, hint, legend, aria-label
- [ ] At least 4 incident cards with specific sources
- [ ] Data sources box with methodology note
- [ ] More-section with all other studies (5 cards)
- [ ] Standard footer with all studies + Credits link and CC badge
- [ ] `index.html` updated with new study card
- [ ] All other study pages' navs, footers, and more-sections updated
- [ ] `credits.html` updated with new study's dataset entry
- [ ] Tested in browser at 375px, 768px, and 1080px widths
- [ ] No broken internal links
