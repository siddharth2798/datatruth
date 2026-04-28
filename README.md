# DataTruth

**Public money, made legible.**

DataTruth is an open-source data journalism project that takes Indian government fiscal data — technically public, practically invisible — and turns it into interactive studies every citizen can read, share, and act on.

🌐 **Live site:** [siddharth2798.github.io/datatruth](https://siddharth2798.github.io/datatruth)

---

## Studies

| # | Study | Key Finding |
|---|-------|-------------|
| 01 | [The March Rush](march-rush.html) | 22–25% of annual welfare budgets spent in the last 30 days of March, every year |
| 02 | [Your MP's ₹5 Crore](mplads.html) | ₹528 crore in MPLADS funds lapsed unspent in FY 2023–24 |
| 03 | [The Absent MP](absent-mp.html) | 62 MPs attended fewer than half of Lok Sabha sessions; 38 asked zero questions |
| 04 | [The Debt Your State Is Hiding](state-debt.html) | Off-budget borrowings inflate real state debt by 5–18 percentage points |
| 05 | [The Scheme Maze](scheme-maze.html) | 15 of 133 CSS schemes consume 91% of the entire CSS budget |
| 06 | [The Pension Timebomb](pension-bomb.html) | OPS-reverting states projected to spend 28% of revenue on pensions by 2040 |

---

## Tech Stack

DataTruth is intentionally minimal. No build step. No framework. No backend.

| Tool | Version | Purpose |
|------|---------|---------|
| HTML5 / CSS3 / JavaScript | — | All pages are self-contained static files |
| [Chart.js](https://www.chartjs.org) | 4.4.3 | All charts and visualisations |
| [Inter](https://rsms.me/inter) | Variable | Body typography (Google Fonts CDN) |
| [Playfair Display](https://fonts.google.com/specimen/Playfair+Display) | 700 | Display headings (Google Fonts CDN) |
| [jsDelivr](https://www.jsdelivr.com) | — | CDN for Chart.js |

Zero npm dependencies. Zero build step. Clone and open `index.html`.

---

## Running Locally

```bash
git clone https://github.com/siddharth2798/datatruth.git
cd datatruth
open index.html          # macOS
# or: xdg-open index.html   # Linux
# or: start index.html       # Windows
```

For a local server (avoids any cross-origin issues with file://):

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

No npm install, no build, no environment variables required.

---

## Project Structure

```
datatruth/
├── index.html              # Homepage — study listing
├── march-rush.html         # Study 01: The March Rush
├── mplads.html             # Study 02: Your MP's ₹5 Crore
├── absent-mp.html          # Study 03: The Absent MP
├── state-debt.html         # Study 04: The Debt Your State Is Hiding
├── scheme-maze.html        # Study 05: The Scheme Maze
├── pension-bomb.html       # Study 06: The Pension Timebomb
├── credits.html            # Open source attribution & data sources
├── docs/
│   ├── architecture.md     # Design system & code conventions
│   └── adding-a-study.md  # How to contribute a new study
├── CONTRIBUTING.md         # Contribution guidelines
├── LICENSE                 # GNU General Public License v3
└── .gitignore
```

---

## Data Sources

All data is sourced from official Government of India publications:

- **CGA** — Controller General of Accounts: monthly union expenditure ([cga.nic.in](https://cga.nic.in))
- **MoSPI** — MPLADS Portal: MP-wise fund data ([mplads.mospi.gov.in](https://mplads.mospi.gov.in))
- **CAG** — Comptroller and Auditor General: audit reports ([cag.gov.in](https://cag.gov.in))
- **PRS Legislative Research** — Parliament Track: MP attendance ([prsindia.org/parliamenttrack](https://prsindia.org/parliamenttrack))
- **RBI** — State Finances: A Study of Budgets ([rbi.org.in](https://rbi.org.in))
- **NIPFP** — Working papers on fiscal policy ([nipfp.org.in](https://nipfp.org.in))
- **PFRDA** — Pension fund regulatory data ([pfrda.org.in](https://pfrda.org.in))
- **data.gov.in** — Open Government Data Platform ([data.gov.in](https://data.gov.in))
- **Union Budget Statement 16** — CSS allocations ([indiabudget.gov.in](https://indiabudget.gov.in))

> **Note on indicative data:** Where exact per-MP or per-state breakdowns are not officially published, data is scaled from published aggregate patterns and clearly labelled with a yellow banner on each study page. All key findings are directly sourced from official publications. See [credits.html](credits.html) for the full methodology note.

---

## Contributing

We welcome contributions — new studies, data corrections, design improvements, and documentation.

See **[CONTRIBUTING.md](CONTRIBUTING.md)** for the full guide.

Quick summary:
1. Fork the repo
2. Create a branch: `git checkout -b study/your-study-name`
3. Follow the study template in `docs/adding-a-study.md`
4. Open a pull request with a description of the data source and key finding

---

## License

DataTruth is free software: you can redistribute it and/or modify it under the terms of the **GNU General Public License v3.0** as published by the Free Software Foundation.

See [LICENSE](LICENSE) for the full text.

The underlying government data (CGA, CAG, MoSPI, RBI, PRS, etc.) is in the public domain under Section 52 of the Indian Copyright Act, 1957. No licence restriction applies to the raw government data — only to DataTruth's original analysis, code, and presentation layer.

---

## Acknowledgements

- [PRS Legislative Research](https://prsindia.org) — for making parliamentary data analysable
- [CAG of India](https://cag.gov.in) — for three decades of consistent independent audit reporting
- [NIPFP](https://nipfp.org.in) — for open-access fiscal policy research
- [Chart.js](https://www.chartjs.org) contributors — for a powerful, free charting library
- [Rasmus Andersson](https://rsms.me) — for the Inter typeface
- The Reporters' Collective, IndiaSpend, The Wire — for establishing that government data analysis is journalism
