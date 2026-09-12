# Read the Tape

A trading game in one HTML file. A ▲ headline usually means up, a ▼ headline
usually means down, and sometimes it lies. Buy when it goes up, sell when it
goes down, flat when unsure. At the end you get a rating from -5 to 5 stars.

**[Play it](https://fazleras.github.io/market-maker/)**. No build, no backend,
nothing to install.

## How to play

| Input | Action |
|---|---|
| `BUY` or `↑` | go long 100 shares |
| `FLAT` or `space` | close out |
| `SELL` or `↓` | go short 100 shares |
| `R` | restart |

Each round is a random stock somewhere between $80 and $400. Every switch
costs the spread, 2 basis points of the price, so about $4 a flip on a $200
stock. The chart turns green while you are long and red while you are short,
so the finished round shows exactly where you were right.

Pick a length (60 seconds or 2 minutes) and a mode:

| Mode | Fakeouts | Silent dumps | Headlines |
|---|---|---|---|
| Calm | 18% | 10% | every 5 to 10 s |
| Normal | 35% | 30% | every 4 to 8 s |
| Chaos | 50% | 45% | every 2.5 to 5.5 s |

## What a headline can do

- **Real.** The price moves 1.5% to 4.5% the way the headline says, over 1.5
  to 6 seconds, in a random shape: a spike that fades, a slow build that
  accelerates, an S-curve, an overshoot that gives some back, a staircase, or
  a straight line. Between headlines the price drifts and wiggles on its own.
- **Fakeout.** It starts the right way, then reverses through where it began.
- **Dud.** Headline hits, nothing happens.
- **Silent dump.** A real move finishes, and a few seconds later the price
  goes the other way with no headline at all. Good news that sells off
  anyway, like the normal market.

The only reliable tell is the line. If it stalls, the move is over.

## The rating

Your P&L is compared with perfect hindsight: a trader who, on the same
price path, holds the true side of every move, is flat in between, pays the
same spread, and still needs 300 ms to act, the same as you. Match it and
you get 5 stars. Lose as much as it made and
you get -5. Each star is 20% of the way.

| Stars | Title |
|---|---|
| 5 | Tape Reader |
| 4 | Sharp |
| 3 | Solid |
| 2 | Getting There |
| 1 | Scratched a Profit |
| 0 | Flat |
| -1 | Paid the Spread |
| -2 | Chased the Headlines |
| -3 | Fooled by the Fakeouts |
| -4 | Bag Holder |
| -5 | Liquidated |

The stars pop in after a short rating animation with a one-line roast, or
praise if you earned it. The round also reports calls right, your best streak
of correct calls, and your best rating for that mode and length.

## Hard mode

[`hard.html`](hard.html) is the same idea with the training wheels off. You
are the market maker, quoting a bid and an ask into a live order book with
price-time priority, against passive quoters, noise flow, and an informed
trader who sweeps your stale quote 150 ms after the headline unless you pull
it first. It has its own matching engine. It is a lot to take in over 60
seconds, which is why it is not the front door.

## Run locally

Open `index.html`. Or `python3 -m http.server` and visit `localhost:8000`.
