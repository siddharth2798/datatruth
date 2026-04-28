# Architecture & Design System

DataTruth is a static HTML site with no build step. This document describes the design system, code conventions, and structural patterns used across all pages.

---

## File Structure

Each study is a self-contained `.html` file. There is no shared CSS file, no templating engine, and no JavaScript module system. Each file embeds:

1. `<style>` — all CSS for that page
2. `<script src="chart.js CDN">` — Chart.js via jsDelivr
3. Inline `<script>` — all chart data and rendering logic

This makes the project trivially hostable (any static host, GitHub Pages, Netlify, file:// protocol) and ensures no page breaks when another page changes.

---

## Design System

### Colours (CSS custom properties)

```css
:root {
  --navy:   #0f172a;   /* nav, hero, more-section, footer backgrounds */
  --blue:   #1d4ed8;   /* interactive elements, blue study cards */
  --red:    #dc2626;   /* primary accent, red study cards, alert */
  --orange: #ea580c;   /* secondary alert, warnings */
  --green:  #16a34a;   /* positive / good outcomes */
  --bg:     #f8fafc;   /* page background */
  --card:   #ffffff;   /* card/box backgrounds */
  --border: #e2e8f0;   /* all borders */
  --txt:    #0f172a;   /* primary text */
  --txt2:   #475569;   /* secondary / body text */
  --txt3:   #94a3b8;   /* labels, metadata, tertiary */
}
```

### Typography

| Element | Font | Weight | Size |
|---------|------|--------|------|
| Logo | Playfair Display | 700 | 1.4rem |
| Hero `h1` | Playfair Display | 700 | `clamp(2rem,4vw,3rem)` |
| Section titles | Playfair Display | 700 | `clamp(1.5rem,2.5vw,2rem)` |
| Body text | Inter | 400 | 0.9375rem (15px) |
| Labels/metadata | Inter | 600–700 | 0.7rem–0.75rem |
| Stat values | Inter | 700 | 1.875rem |

### Spacing

Uses a 0.125rem (2px) grid. Common values: `0.375rem`, `0.5rem`, `0.625rem`, `0.875rem`, `1rem`, `1.25rem`, `1.5rem`, `2rem`, `3.5rem`.

---

## Page Anatomy

Every study page follows this structure in order:

```
<head>
  meta charset, viewport
  title
  meta description + OG tags
  Google Fonts preconnect + stylesheet
  Chart.js CDN script
  <style> (all page CSS)
</head>
<body>
  <div id="progress">         ← reading progress bar (fixed, top)
  <nav>                       ← sticky navigation
  <div class="notice">        ← yellow data-quality banner
  <section class="study-hero"> ← dark hero: tag, h1, description, analogy
  <div class="page-body">
    <div class="container">
      summary-box             ← 3–5 bullet findings
      stat-grid               ← 5–6 stat cards
      chart-card(s)           ← charts with hints and legends
      insight                 ← red insight callout
      [additional charts]
      incidents section       ← documented real-world consequences
      ds-box                  ← data sources + methodology note
    </div>
  </div>
  <div class="more-section">  ← navy band: links to 5 other studies
  <footer>                    ← consistent footer with all study links
  <button id="btt">           ← back-to-top (fixed, bottom-right)
  <script>                    ← Chart.js data + rendering
  scroll/progress JS
</body>
```

---

## Chart Conventions

All charts use Chart.js 4.4.3. Each chart block follows this pattern:

```html
<div class="chart-card">
  <div class="chart-head">
    <div class="chart-title">Descriptive title of what is being shown</div>
    <div class="chart-sub">Data source, year range, unit</div>
  </div>
  <div class="chart-hint">
    <span>💡</span>
    <span class="hint-text">How to read this: plain-language explanation of the chart type and what to look for</span>
  </div>
  <div class="legend-row">
    <!-- legend items if needed -->
  </div>
  <canvas id="chartN" height="120" role="img" aria-label="[descriptive label]"></canvas>
</div>
```

### Chart types by use case

| Use case | Chart type |
|----------|-----------|
| Ranking (states, MPs, schemes) | Horizontal bar (`indexAxis: 'y'`) |
| Trend over time | Line chart |
| Monthly distribution | Vertical bar |
| Composition (debt breakdown) | Stacked horizontal bar |
| Correlation (attendance vs questions) | Scatter |
| History with two metrics | Mixed bar + line |

### Custom plugins

For reference lines (FRBM targets, NPS averages, equal-distribution lines), use a Chart.js inline plugin passed to the `plugins` array:

```js
{
  id: 'refLine',
  afterDraw(chart) {
    const { ctx, scales } = chart;
    const x = scales.x.getPixelForValue(25);  // e.g. 25% FRBM target
    ctx.save();
    ctx.beginPath();
    ctx.moveTo(x, scales.y.top);
    ctx.lineTo(x, scales.y.bottom);
    ctx.strokeStyle = '#7c3aed';
    ctx.lineWidth = 1.5;
    ctx.setLineDash([4, 3]);
    ctx.stroke();
    ctx.restore();
  }
}
```

---

## Multi-year Interactive Data Pattern

Used on `march-rush.html` and `mplads.html`. The pattern:

```js
const YEAR_META = {
  '2023-24': { scale: 1.00, label: null, type: null },
  '2022-23': { scale: 0.95, label: 'Context note', type: 'alert' },
  '2020-21': null,   // suspended year — shows warning panel, hides charts
};

function renderAll(year) {
  const meta = YEAR_META[year];
  if (!meta) { showSuspendedPanel(); return; }
  updateStatCards(year, meta);
  updateCharts(year, meta);
  updateContextBanner(meta);
}

document.getElementById('yearSelect').addEventListener('change', e => {
  renderAll(e.target.value);
});
```

`null` in `YEAR_META` means the year has no data (e.g. MPLADS was suspended in 2020–22). The page shows a red warning panel explaining why rather than showing misleading zeroes.

---

## Accessibility

- All `<canvas>` elements must have `role="img"` and a descriptive `aria-label`
- Navigation links use `.active` class on the current page
- Colour is never the sole means of conveying information (legends always have text labels)
- Contrast: primary text `--txt` (#0f172a) on `--bg` (#f8fafc) = 18.1:1 (AAA)
- Back-to-top button has `aria-label="Back to top"`
- Progress bar has `pointer-events:none` to avoid focus issues

---

## Performance Notes

- No JavaScript framework: ~0 KB of framework overhead
- Chart.js 4.4.3 UMD: ~200 KB (served from jsDelivr with edge caching)
- Google Fonts: 2 families, preconnect optimised, subset via display=swap
- Total page weight: typically 250–350 KB including Chart.js
- All assets are served from CDN; no assets are bundled or self-hosted
