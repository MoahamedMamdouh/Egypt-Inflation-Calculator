# Egypt Inflation Calculator

> **Live:** <https://egypt-inflation-calculator.devlabtechnologies.com>

A bilingual (Arabic / English, RTL-aware) web calculator that answers
the single most awkward question in any high-inflation economy:

> *"My money from a few years ago — what is it actually worth now?"*

You enter an EGP amount and a past year, and the app runs four parallel
scenarios so you can compare what your money *did* buy vs. what the
same number of pounds is worth today.

![Screenshot](https://egypt-inflation-calculator.devlabtechnologies.com/og.png)

---

## What it calculates

For every amount + year you enter, the app computes four outcomes:

| Scenario | Logic |
| --- | --- |
| 💸 **Inflation-adjusted** | `amount × CPI(now) / CPI(then)` — how many EGP you'd need today to match the original purchasing power. |
| 🥇 **Gold** | Convert EGP → USD at the historical rate, buy gold at that year's LBMA price, sell at today's price, convert back to EGP. |
| ₿ **Bitcoin** | Same as gold, using BTC price instead. Unavailable for years before 2010. |
| 💵 **Foreign-currency savings** | Convert EGP → USD / EUR / GBP / SAR / AED at the historical rate, hold as cash (no interest), re-value today. |

Each scenario card shows:

- **Worth today** in EGP
- **Gain / loss** vs. the original nominal amount
- **CAGR** — annualised return
- **Multiplier** — how many times the money grew in nominal terms
- **Real multiplier vs. inflation** — the one that actually matters

Anything under `1.0×` in that last row means the scenario *lost*
purchasing power despite nominal growth — a common outcome for
"saving EGP under the mattress" and an explicit design goal of the
calculator to surface.

---

## Features

- 🇪🇬 **Arabic default, English secondary** — full RTL layout, Tajawal for Arabic, Geist for Latin, persists via `localStorage`.
- 📴 **Works offline, no API keys** — all historical data (1990–2026) is bundled at build time.
- 📱 **Mobile-first** — form fields stack on small screens; result cards reflow gracefully.
- ♿ **Bidi-safe** — copyright and mixed-language labels use `<bdi>` isolates so reading order stays natural in both directions.
- 🌙 **Dark mode** — honors `prefers-color-scheme`.
- ⚡ **Static export** — one HTML file and some JS; serves from any CDN with no runtime.

---

## Data sources

All figures are **annual averages** bundled in [`lib/historical-data.ts`](lib/historical-data.ts).
They're intended for comparison, not as live financial quotes.

| Series | Source |
| --- | --- |
| Egypt CPI & YoY inflation | IMF WEO, World Bank, CAPMAS |
| USD/EGP, EUR/EGP, GBP/EGP | IMF IFS, Central Bank of Egypt averages |
| SAR/EGP, AED/EGP | Derived from USD/EGP via the SAR and AED pegs (3.75 and 3.6725) |
| Gold (USD / troy oz) | LBMA PM Fix, annual average |
| Bitcoin (USD) | CoinGecko / historical market averages |

Updating for a new year is a one-file change: append an entry to each
series in [`lib/historical-data.ts`](lib/historical-data.ts) and bump
`CURRENT_YEAR`.

---

## Tech stack

- [**Next.js 16**](https://nextjs.org) (App Router, `output: "export"`)
- [**TypeScript**](https://www.typescriptlang.org) with `strict`
- [**Tailwind CSS v4**](https://tailwindcss.com)
- [**Tajawal**](https://fonts.google.com/specimen/Tajawal) +
  [**Geist**](https://vercel.com/font) via `next/font/google`
- Hosted on [**Cloudflare Pages**](https://pages.cloudflare.com) with auto-deploy from `main`

Because the app is 100% client-side — no server actions, no API routes
— it ships as plain HTML/CSS/JS and runs on any static host.

---

## Project layout

```
.
├── app/
│   ├── globals.css        # Tailwind v4 + RTL/LTR font rules
│   ├── icon.svg           # DevLab favicon
│   ├── layout.tsx         # html lang/dir, Tajawal + Geist fonts
│   └── page.tsx           # full calculator UI + i18n strings
├── lib/
│   ├── calculate.ts       # pure calculation engine + locale formatters
│   └── historical-data.ts # CPI, FX, gold, BTC series
├── next.config.ts         # output: "export"
└── README.md
```

---

## Local development

```bash
npm install
npm run dev        # http://localhost:3000
```

```bash
npm run build      # produces ./out as a static site
npm run lint
```

---

## Deployment

Every push to `main` triggers a build on Cloudflare Pages:

| Setting | Value |
| --- | --- |
| Framework preset | Next.js (Static HTML Export) |
| Build command | `npm run build` |
| Build output | `out` |
| Production branch | `main` |
| Custom domain | `egypt-inflation-calculator.devlabtechnologies.com` |

---

## Contributing

Pull requests welcome — especially for:

- Refreshing the historical series with newly published annual averages
- Adding additional savings currencies (e.g. KWD, CHF)
- Translating the UI into more languages beyond Arabic/English

Please keep the calculation logic in [`lib/calculate.ts`](lib/calculate.ts)
free of presentation / string concerns — it's intentionally pure so it
stays language-agnostic.

---

## Credits

Built by **[AHMED ELKHAYYAT](https://elkhayyat.me)**
· [𝕏](https://x.com/AHMEDELKHAYYAT)
· [LinkedIn](https://linkedin.com/in/elkhayyat)

Sponsored by **[DevLab Technologies LLC](https://devlabtechnologies.com)**.

---

## License

MIT — see [`LICENSE`](LICENSE) if present, otherwise treat as MIT.
