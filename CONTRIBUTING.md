# Contributing to DataTruth

Thank you for your interest in contributing. DataTruth is an open-source data journalism project — every contribution, whether it's a data correction, a new study, a design improvement, or a documentation fix, makes the project more useful for citizens.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Ways to Contribute](#ways-to-contribute)
- [Before You Start](#before-you-start)
- [Adding a New Study](#adding-a-new-study)
- [Fixing Existing Data](#fixing-existing-data)
- [Design & UX Changes](#design--ux-changes)
- [Pull Request Process](#pull-request-process)
- [Data Standards](#data-standards)
- [Code Style](#code-style)

---

## Code of Conduct

DataTruth analyses how public money is spent. Contributions must be:

- **Factual** — grounded in official government data or peer-reviewed research
- **Non-partisan** — the project holds all governments accountable equally, regardless of party
- **Transparent** — every finding must cite a primary source that can be independently verified
- **Accessible** — analysis should be readable by a citizen with a secondary education, not just economists

---

## Ways to Contribute

### 🐛 Report a data error
Open an issue with the label `data-error`. Include:
- Which page/chart is incorrect
- What the current value shows
- What the correct value should be
- A link to the primary source

### 📊 Propose a new study
Open an issue with the label `new-study`. Include a one-paragraph description of:
- The accountability question being asked
- The dataset that answers it (with a direct link)
- The key finding you expect the data to reveal

### 🎨 Fix a UI/UX issue
Open an issue with the label `ux`. Screenshots and browser/device details appreciated.

### 📝 Improve documentation
PRs fixing typos, improving explanations, or adding clearer methodology notes are always welcome — no issue needed.

---

## Before You Start

1. **Check existing issues** — someone may already be working on it
2. **Use real data** — DataTruth does not publish fabricated or unverifiable statistics
3. **One study per PR** — keep changes focused and reviewable
4. **No build step** — the project is pure HTML/CSS/JS; do not introduce npm, webpack, or any bundler

---

## Adding a New Study

See the full template guide in **[docs/adding-a-study.md](docs/adding-a-study.md)**.

### Quick checklist

- [ ] Study answers a specific accountability question about Indian public finance
- [ ] All data is from an official Government of India source or peer-reviewed institution
- [ ] Data provenance is documented in the `ds-box` section at the bottom of the page
- [ ] Indicative/modelled data is labelled with the yellow `.notice` banner
- [ ] Charts include a `.chart-hint` "How to read this" box
- [ ] Page has a `.summary-box` at the top with 3–5 bullet-point findings
- [ ] Page has 4–6 `.incident-card` sections documenting real-world consequences
- [ ] Page links to all other studies in the `.more-section` at the bottom
- [ ] Nav includes all existing study links with correct `.active` class
- [ ] Footer follows the standard template (see any existing study page)
- [ ] Progress bar and back-to-top button are included (see any existing study page)
- [ ] Meta description and OG tags are set in `<head>`
- [ ] New study is added to `index.html` study cards
- [ ] New study is added to the nav and footer of all other study pages
- [ ] New study is linked in `credits.html` under "Datasets Used, Study by Study"

---

## Fixing Existing Data

If you are correcting a data value:

1. Update the JavaScript data array in the HTML file
2. Update any stat cards that reference the same figure
3. Update the `ds-box` source if the correction comes from a different document
4. In your PR description, link to the primary source document page/paragraph

Data corrections do not need a new issue first — just open a PR.

---

## Design & UX Changes

DataTruth uses a CSS custom property design system defined in each page's `<style>` block:

```css
:root {
  --navy:#0f172a;   /* primary dark background */
  --blue:#1d4ed8;   /* interactive / secondary accent */
  --red:#dc2626;    /* primary accent, alert */
  --orange:#ea580c; /* secondary alert */
  --green:#16a34a;  /* positive / good */
  --bg:#f8fafc;     /* page background */
  --card:#ffffff;   /* card background */
  --border:#e2e8f0; /* borders */
  --txt:#0f172a;    /* primary text */
  --txt2:#475569;   /* secondary text */
  --txt3:#94a3b8;   /* tertiary / label text */
}
```

**Do not** introduce new color values outside this system without discussion.

**Typography:** Inter for body text, Playfair Display for headings and the logo. Do not add additional font families.

**No CSS frameworks** (Bootstrap, Tailwind, etc.) — the project uses hand-written CSS intentionally for performance and hostability.

---

## Pull Request Process

1. Fork the repository and create a branch:
   ```bash
   git checkout -b study/your-study-name
   # or: git checkout -b fix/data-correction-description
   # or: git checkout -b ux/improvement-description
   ```

2. Make your changes. Keep commits focused — one logical change per commit.

3. Test by opening the modified HTML files in a browser. Check:
   - Charts render correctly
   - The page is readable on mobile (375px viewport)
   - No broken internal links
   - All data source links open correctly

4. Open a pull request with:
   - A one-sentence description of what changed
   - A link to the primary data source (for data changes)
   - Screenshots for any visual changes

5. PRs are reviewed within 7 days. Feedback will focus on data accuracy and source quality, not stylistic preferences.

---

## Data Standards

| Standard | Requirement |
|----------|-------------|
| **Primary sources only** | Data must come from a GOI publication, RBI report, or peer-reviewed institution. News articles can be cited in incident cards but not as primary data sources. |
| **Indicative data labelling** | If exact figures are unavailable, label the data with the yellow `.notice` banner and explain the methodology in the `.method-box`. |
| **No imputation without disclosure** | If values are modelled or interpolated, explain the model in the methodology note. |
| **Financial years** | Use Indian financial year format: `2023–24` (en-dash, not hyphen). |
| **Currency** | Use Indian number system: ₹ crore, ₹ lakh crore. Avoid converting to USD. |
| **Rounding** | Round to 1 decimal place for percentages; 0 decimal places for whole crore figures. |

---

## Code Style

- **No comments** in HTML/CSS/JS unless the reason is non-obvious (a browser quirk workaround, a non-obvious invariant)
- **Inline styles** are acceptable for one-off per-element values; prefer classes for repeated patterns
- **JavaScript** — vanilla only, no libraries except Chart.js. Use `const`/`let`, not `var`. Use IIFEs to scope chart code
- **Chart.js** — always set `responsive: true`. Always include `aria-label` and `role="img"` on `<canvas>` elements
- **HTML** — use semantic elements where appropriate (`<section>`, `<nav>`, `<footer>`, `<article>`)
- **Self-contained files** — each study page must work as a standalone file with no external JS dependencies other than Chart.js and Google Fonts

---

## Questions?

Open an issue with the label `question`. Response time: typically within a few days.
