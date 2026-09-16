
# Inspired by this Illustration!
<img width="1778" height="790" alt="CRT-MODEL-PRO" src="https://github.com/user-attachments/assets/69ebe1bb-0399-4a24-ab50-8add6a82e7ef" />

# CRT Model Pro — GXV-SMT + CRT (Ultimate Build)

Pine Script v6 indicator (`overlay=true`). This is a single merged script combining
the original CRT/SMT/CISD engine with a namespaced True Opens layer. It supersedes
`GXV-SMT_CRT_ULTIMATE_tCISD_TRUEOPENS_DASH.pine`.

File: `GXV-SMT_CRT_ULTIMATE_FINAL.pine`

---

## What's in it

| Engine | Status | Source |
|---|---|---|
| CRT core (HTF mapping, candle build, separators) | Original, untouched | Sam base file |
| ICT / QT Alignment (single toggle) | Original, untouched | Sam base file |
| SMT (multi-rank correlated-pair divergence) | Original, untouched | GXV base file |
| CISD detection + Fibonacci projections | Original, untouched | Sam base file |
| True Opens (Session / Day / Week / Month / Year) | **Replaced** | Daye's Quarterly Theory — True Opens (open-source) |
| HTF candle preview panel + dashboard | Original, untouched | Sam base file |
| tCISD, Turtle Soup, CRT H/L | **Removed** | — |

---

## True Opens

Ported in as a namespaced block (`crtTo_` prefix) rather than a separate indicator.
Behavior is unchanged from the source script:

- **One live line + label per type.** A new period's open replaces the previous
  line for that type — it does not accumulate history.
- Lines extend live to the current bar every bar; labels sit to the right by
  `Label Gap` bars.
- Fixed `America/New_York` timezone (matches the source script; not tied to the
  CRT engine's own timezone switch).
- Types: **Session** (Asia 19:30, London 01:30, NY AM 07:30, NY PM 13:30 — off by
  default), **Day** (00:00 NY), **Week** (Mon 18:00 NY), **Month** (2nd Monday
  18:00 NY), **Year** (April 1, 00:00 NY — Q2 of the yearly cycle).
- No 90-minute Open (the source script doesn't have one; the old True Opens
  engine's 90m Open was dropped along with it).
- All five types default to **black**; each has its own color input.
- Labels default to `size.small`.
- Inputs live under one **True Opens** group: visibility per type, per-type
  colors, line width/style, label gap, and a labels on/off toggle.

## Dashboard / HTF panel

Top-right compact table: **CRT Model (timeframe)**, **QT Alignment** mode,
Model, Bias, SMT pair, Date. The timeframe shown (both in the table header and
in the "TF: … [ICT/QT]" label on the HTF candle panel) is formatted for
readability — e.g. `6H`, `90m`, `1D` — instead of Pine's raw internal string
(`360`, `90`, `D`). This is display-only; the raw string is still what drives
the actual timeframe-mapping logic.

The HTF candle preview panel (the small candle stack to the right of price)
draws directly from the same aggregated HTF OHLC arrays the CRT engine itself
maintains — there's no separate/duplicate data path for it.

## SMT

Untouched apart from:
- Automatic ICT/QT timeframe mapping (shares the CRT engine's single active-timeframe
  calculation — no second hierarchy).
- Default **SMT Text** color changed to black (was dark gray).

## CISD

Untouched. Always active — the earlier `tCISD Mode` toggle that could hide it
has been removed, so CISD detection and its Fibonacci projections run
unconditionally per their own inputs.

---

## What was removed

**tCISD, Turtle Soup, and CRT H/L are gone.** These were a from-scratch build
(the tCISD "core" was never a faithful port of the SxM Blitz source, and one
real bug was found and fixed along the way — the reference-candle search offset
was accidentally wired to an unrelated Turtle Soup input — before the whole
feature set was removed). Every input, alert, and drawing call tied to them has
been deleted, along with now-orphaned helper variables that existed only to
feed them. Nothing else was touched by their removal — CRT, SMT, CISD, True
Opens, and the dashboard are unaffected.

---

## Compiling

Pine v6. Single `indicator()` declaration. Compile in TradingView's Pine Editor
and check for errors before relying on it live — this file has been reviewed
line-by-line but not run against live/replay data.
