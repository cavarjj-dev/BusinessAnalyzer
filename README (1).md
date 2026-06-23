# Rule №1 — The Four M's Ledger

A single-file value-investing desk in the Buffett / Munger / Phil Town tradition.
Score companies on **Meaning · Moat · Management · Margin of Safety**, track multiple
portfolios, run Phil Town's sticker-price math, get live quotes + price alerts, and keep a
versioned performance log — hosted free on GitHub Pages, installable as an app, with your
credentials sealed by real client-side encryption.

## Files (commit all of these)

```
Rule1/
├── index.html              ← the whole app
├── service-worker.js       ← offline / PWA cache
├── manifest.webmanifest    ← install-as-app metadata
├── icon.svg                ← app icon
├── data/
│   └── portfolio.json      ← your data (holdings, scores, history)
└── README.md
```

## Deploy (2 minutes)

1. Put `index.html`, `service-worker.js`, `manifest.webmanifest`, `icon.svg` in the repo root and the JSON at `data/portfolio.json`.
2. Repo → **Settings → Pages** → *Deploy from a branch* → `main` / root → **Save**.
3. Open `https://<your-username>.github.io/Rule1/`. On phone: browser menu → **Add to Home Screen** to install it as an app (works offline).

> The service worker and install only work over **https** (GitHub Pages qualifies), not by double-clicking the file locally.

## What's inside

- **Multiple portfolios** — switch between Son's Fund, Personal, etc.; each has its own holdings and performance history.
- **Live quotes** — pick a provider in Settings: **Finnhub** (free key, reliable) or **Stooq** (keyless, best-effort; browsers may CORS-block it). Optional **auto-refresh** on a timer, plus per-holding **price alerts** with **browser notifications**.
- **Sticker price & MOS** — Phil Town's method per company; **Big Five** checked against the 10% floor; verdict stamp: Wonderful / Wait / Avoid / Too Hard.
- **Performance log** — every Snapshot / Refresh / repo-save records a dated value point; see the curve under **Performance**.

## Persistence (so nothing is ever lost)

- **Autosave** to this browser on every edit.
- **Repo as source of truth** — first load reads `data/portfolio.json`; *Reload repo file* re-pulls it.
- **Export / Import** JSON anytime.
- **Save to repo** — commits `data/portfolio.json` via the GitHub API; every commit is a versioned snapshot that travels across devices.

## Security & encryption

- **Real crypto:** secrets are sealed with **AES-256-GCM**, key derived via **PBKDF2 (SHA-256, 250k iterations)** using the browser's Web Crypto API. Your passphrase is **never stored**. You can also encrypt the whole portfolio at rest (unlock required to view).
- **Token hygiene:** a GitHub token is persisted **only** inside the encrypted vault. With encryption off, it's session-only (memory, gone on close). **Auto-lock** clears secrets after inactivity.
- **Network lockdown:** a **Content-Security-Policy** limits connections to Finnhub, GitHub, Stooq, and the CDN only; `object-src 'none'`, `base-uri 'self'`, no inline forms, `referrer: no-referrer`.
- **Token advice:** use a **fine-grained** GitHub token scoped to *only* the Rule1 repo, **Contents: read/write**, with a short expiry.

**Honest limits:** client-side encryption protects data *at rest in this browser*. It cannot protect a compromised device, malware/keyloggers, or anyone who has your passphrase. For maximum CDN-supply-chain hardening, vendor the React/Babel files into the repo and add Subresource Integrity (SRI) hashes.

## The workflow

1. Read a 10-K / annual report **with Claude in chat** → pull the Big Five and draft the four M's.
2. Enter the numbers and scores here → the ledger stamps a verdict.
3. Snapshot or Save to repo to log performance over time.

*Not investment advice. Your scorecard and system of record — the judgment is yours.*
