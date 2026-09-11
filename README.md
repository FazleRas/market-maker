# Market Maker

A 60-second market-making game in one HTML file. You quote a bid and an ask in
one stock; noise traders hit whichever quote is best and you pocket the spread.
Meanwhile a hidden fair value drifts, competing quoters undercut you, and when
news breaks an informed trader sweeps stale quotes before you can move. At the
buzzer you are flattened at market, so inventory you are still holding costs
you the spread.

**[Play it](https://fazleras.github.io/market-maker/)** — no build, no backend,
nothing to install.

## How to play

| Input | Action |
|---|---|
| click the ladder | left half places your bid at that price, right half your ask |
| `↑` / `↓` | tighten / widen both quotes by a tick |
| `←` / `→` | skew both quotes down / up (lean inventory off) |
| `space` (hold) | pull both quotes |
| `R` | restart |

Quotes are 100 shares and are expressed as offsets from the market's mid, so
they follow the price — but the peg only refreshes every 600 ms on its own,
or the instant you touch a control. That is the whole game: the competing
quoters reprice within ~100 ms of news and the informed trader is faster
still, so after a headline the slow quote left in the book is yours until you
move it. Inventory is capped at ±300: at the cap, the side that would add to
it stops quoting. After a quote is taken in full it stays down for 400 ms
before re-posting.

Things that separate a good round from a bad one:

- **Tight when it is quiet.** The touch is only intermittently quoted by the
  bots, so a quote one tick inside them gets all the noise flow.
- **Gone when it is not.** The news ticker flashes at the same moment fair
  value jumps and the informed trader starts sweeping. Hold space first, think
  second. The sweep ramps up over 1.5 s and stops on its own once the market
  has repriced, so a fast pull saves most of it.
- **Lean inventory off as you go.** Long 300 shares into a downside headline
  is how rounds end badly. Skew your quotes to get flat.

## How it works

Everything runs through one matching engine with price-time priority: limit
and market orders, bots and you alike, in integer cents. The player is never
special-cased in the matching.

Three kinds of bot, on a 50 ms tick:

- **Passive quoters** keep a ladder of resting orders around fair value —
  thicker away from the touch, thin and intermittent at it. Orders that end
  up on the wrong side of fair value are pulled with a ~100 ms lag, orders
  that drift too far with a slower one.
- **Noise traders** send market orders at ~7/s, sizes 10–60, random side.
  This is the flow you are paid to absorb.
- **The informed trader** wakes on each news event, which jumps fair value by
  18–44 cents. Starting 150 ms after the headline it sends market orders in
  the news direction, small probes first and full-size sweeps by the end of
  the 1.8 s window — but only while the touch is still on the wrong side of
  fair value. It never pays above fair value, so the sweep is self-limiting.

Fair value itself is a random walk (σ = 0.25 ¢ per tick) plus the jumps. You
never see it; you see the book, the tape, and the headline.

Scoring: marked P&L (cash + inventory × mid) is shown live. The final score
crosses the bot book to flatten whatever you hold; size the book cannot absorb
clears 40 ¢ through fair value. "Spread earned" is each fill's distance inside
fair value at the moment it happened — the honest measure of whether your
quotes were good, separate from how the position moved afterwards.

## Run locally

Open `index.html`. Or `python3 -m http.server` and visit `localhost:8000`.
