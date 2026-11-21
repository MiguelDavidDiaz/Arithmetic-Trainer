# Zeta Drill

A Zetamac-style mental math trainer that **remembers the problems you miss and brings them back** until you can answer them quickly.

Zetamac gives you random problems every time. Zeta Drill uses the same default game, but it tracks every problem you get wrong or answer slowly and feeds those back into your rounds, spaced-repetition style.

**▶ Live demo:** https://migueldaviddiaz.github.io/Arithmetic-Trainer/

## Features

- **Zetamac defaults:** addition (2–100) + (2–100), subtraction as the reverse of addition, multiplication (2–12) × (2–100), division as the reverse of multiplication, 120-second rounds. Answers submit automatically.
- **Weak list:** a problem goes on the list if you type a wrong answer or go over a slow threshold (3 s by default, adjustable). It comes off after 3 fast, clean solves in a row.
- **Review mix:** a set share of each round (35% by default) comes from your weak list. The slowest problems and the ones you miss most often get picked more.
- **Weak-only drills:** a round made entirely of your misses.
- **Extras (optional):** unit-circle sin/cos/tan in radians or degrees, logs and exponents (`ln 0`, `log₂ 64`, `8^(2/3)`), powers and roots (`x²` to 30, `x³`, `2ⁿ`, `√`, `∛`), polynomials evaluated at a point, and combinatorics (`n!`, `C(n,k)`, `P(n,k)`). Exact answers are typed as `r3/2` for √3/2, `1/2` or `.5`, `u` for undefined and `-inf` for −∞, and on mobile there's an on-screen key row.
- **Solve-time distributions:** a box-and-dot plot for each operation (median, IQR, p10–p90, slow threshold) after every round and across your last 400 solves.
- **Fact-family heatmap:** average time and flag rate for each family: +, −, ×2…×12, ÷2…÷12.
- **Score history:** a chart of every 120-second round with a 5-round moving average.
- **Export / Import:** progress is stored in `localStorage`. Export a JSON file to move it to another device.
- One self-contained `index.html` with no build step and no dependencies. Works on desktop and mobile.

## Run it

Open `index.html` in a browser, or host it on GitHub Pages:

1. Push this repo to GitHub.
2. Go to **Settings → Pages**, choose **Deploy from a branch**, then select `main` and `/ (root)`.
3. The site goes live at https://migueldaviddiaz.github.io/Arithmetic-Trainer/ after a minute or so.

## How it decides what's "weak"

Each problem is stored under a canonical key (so `7 × 86` and `86 × 7` count as the same fact) along with:

| field | meaning |
|---|---|
| `t` | exponential moving average of solve time |
| `flags` | number of times it was slow or answered wrong |
| `err` | number of times a wrong answer was typed |
| `box` | clean-solve streak (Leitner-style); a flag resets it to 0, and 3 or more means it has graduated |

Weak problems are sampled with weight ∝ `(3 − box) × (1 + err/n) × clamp(t / threshold)`. The 6 most recent problems are excluded so the same one doesn't repeat back to back.

## License

MIT
