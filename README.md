# Egypt Inflation Calculator

Enter how much money you had in Egypt in a past year, then see what it
would be worth today — adjusted for inflation, or if you had put it into
gold, Bitcoin, or saved it in a foreign currency (USD by default; EUR,
GBP, SAR, AED also supported).

Live at **<https://egypt-inflation-calculator.devlabtechnologies.com>**.

Available in Arabic (default, RTL) and English.

## What it calculates

For every input, the app runs four parallel scenarios:

| Scenario | Logic |
|---|---|
| **Inflation-adjusted** | `amount × CPI(now) / CPI(then)` — EGP you'd need today for the same buying power. |
| **Gold** | Convert EGP → USD at the historical rate, buy gold at that year's price, sell at today's price, convert back to EGP. |
| **Bitcoin** | Same as gold, using BTC price instead. Unavailable for years before 2010. |
| **Foreign currency savings** | Convert EGP → foreign currency at the historical rate, hold cash (no interest), re-value at today's rate. |

Each result card shows the value today, gain/loss vs. the original
nominal amount, annualised return (CAGR), multiplier, and — the one that
matters most in a high-inflation economy — the **real** multiplier
against CPI. Anything below 1.0× here means the scenario *lost*
purchasing power despite nominal growth.

## Data sources

All figures are **annual averages** bundled at build time, so the app
works offline and without any API keys. Figures are approximations
intended for comparison, not live financial quotes.

- Egypt CPI / inflation: IMF WEO, World Bank, CAPMAS.
- USD/EGP, EUR/EGP, GBP/EGP: IMF IFS, Central Bank of Egypt averages.
- SAR and AED derived from USD/EGP using their USD pegs (3.75 and
  3.6725 respectively).
- Gold: LBMA PM Fix annual average (USD/troy oz).
- Bitcoin: CoinGecko / historical market averages (USD).

Historical data lives in [`lib/historical-data.ts`](lib/historical-data.ts);
the calculation engine is in [`lib/calculate.ts`](lib/calculate.ts).

## Tech stack

- **Next.js 16** (App Router, static export)
- **TypeScript** with strict mode
- **Tailwind CSS v4**
- **Tajawal** (Arabic) + **Geist** (Latin), via `next/font/google`
- **Cloudflare Pages** for hosting (see [Deployment](#deployment))

The app is pure client-side — no server actions, no API routes — which
means it exports to plain HTML/CSS/JS (`output: "export"`) and serves
from any static CDN.

## Local development

```bash
npm install
npm run dev    # http://localhost:3000
```

```bash
npm run build  # produces the static site in ./out
```

## Deployment

The site deploys automatically to Cloudflare Pages on every push to
`main`. Pages project config:

- **Framework preset**: `Next.js (Static HTML Export)`
- **Build command**: `npm run build`
- **Output directory**: `out`
- **Custom domain**: `egypt-inflation-calculator.devlabtechnologies.com`

## Contributing & updating data

As new annual averages become available (CPI, FX, gold, BTC), update
the series in [`lib/historical-data.ts`](lib/historical-data.ts) and
bump `CURRENT_YEAR` if needed. The rest of the app adapts
automatically.

## Credits

Built by **[AHMED ELKHAYYAT](https://elkhayyat.me)** ·
[X](https://x.com/AHMEDELKHAYYAT) ·
[LinkedIn](https://linkedin.com/in/elkhayyat)

Sponsored by **[DevLab Technologies LLC](https://devlabtechnologies.com)**.
