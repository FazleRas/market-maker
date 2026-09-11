# Read the Tape

A 60-second trading game in one HTML file. A stock trades in front of you;
every few seconds a headline hits and the price moves — usually the way the
headline says, sometimes not. Go long, go short, or stay flat. At the end you
see how much of the available move you actually caught.

**[Play it](https://fazleras.github.io/market-maker/)** — no build, no backend,
nothing to install.

## How to play

| Input | Action |
|---|---|
| `BUY` · `↑` · `B` | go long 100 shares |
| `FLAT` · `space` · `F` | close out |
| `SELL` · `↓` · `S` | go short 100 shares |
| `R` | restart |

Every switch costs the spread (2 ¢ a share, $2 a flip), so flapping between
buttons loses money on its own. The chart shades green while you are long and
red while you are short, so the finished round is a picture of where you were
right.

One headline in four is a fake-out: the price starts the way the headline
says, then reverses through where it began. The tell is the line, not the
text — if the move stalls, it is not going.

## The score

**Market-reading score** is your P&L as a percentage of a hindsight oracle's:
a trader who, on the same price path, holds the true direction of every
headline for its whole move (including the reversal of a fake-out), is flat in
between, and pays the same spread. 100% means you caught everything there was
to catch. The results also show buy & hold, headlines called right, average
reaction time from headline to being on the right side, and what you paid in
spread.

## How it works

Price is a random walk (σ = 1 ¢ per 50 ms tick) plus headline moves: each
headline pushes the price 25–70 ¢ over 2–5 seconds at a constant rate. A
fake-out pushes for the first 35% of that window and then reverses at 1.4×.
Headlines are spaced so that moves never overlap.

## Hard mode

[`hard.html`](hard.html) is the same idea with the training wheels off: you
are the market maker, quoting a bid and an ask into a live order book with
price-time priority, against passive quoters, noise flow, and an informed
trader who sweeps your stale quote 150 ms after the headline unless you pull
it first. It has its own matching engine and its own calibration story; see
the source. It is a lot to read in 60 seconds, which is why it is not the
front door.

## Run locally

Open `index.html`. Or `python3 -m http.server` and visit `localhost:8000`.
