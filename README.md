# CRT Model Pro — CRT by Sam (with GXV SMT)

Pine Script v6 indicator (`overlay=true`). A single merged script combining
Sam's own CRT/CISD/dashboard engine with one embedded third-party module
(GXV SMT) and one embedded open-source layer (True Opens).

File: `GXV-SMT_CRT_ULTIMATE_FINAL.pine`

![CRT Model Pro reference](assets/crt-model-pro-inspiration.png)

*The layout above — the LTF chart on the left, the small HTF candle preview
stack on the right, the compact top-right dashboard, and the CRT H/L levels
drawn as clean horizontal lines — is the visual reference this build is
inspired by and built to match.*

---

## Attribution — who actually wrote what

This matters, so it's stated plainly, and the same notice is repeated as a
comment block inside the `.pine` file itself, directly above the SMT section:

| Component | Author | License / terms |
|---|---|---|
| CRT engine (HTF mapping, candle build, separators, layout, PSP correlation, trace lines, labels, session alignment) | **Sam** | Original work |
| CISD detection + Fibonacci projections | **Sam** | Original work |
| Dashboard | **Sam** | Original work |
| **SMT Divergences module** | **Gregorius_XV** (`GXV SMT_Divergences[LITE✦]`) | © 2026 Gregorius_XV — **Mozilla Public License 2.0**. The original author's attribution notice requires visible credit to Gregorius_XV be preserved in any redistribution, fork, derivative, port, or modified publication based on it. That notice is kept intact, verbatim, in the `.pine` file immediately above the SMT code, and is not to be removed. |
| True Opens (Session/Day/Week/Month/Year) | **Daye** (`Daye's True Opens [Quarterly Theory]`) | Open-source, used with author confirmation |

Everything that isn't the SMT module or the True Opens block is Sam's own
work — the CRT core, CISD, and the dashboard are not ports of anyone else's
script.

---

## What's in it

| Engine | Status | Source |
|---|---|---|
| CRT core (HTF mapping, candle build, separators) | Original | Sam |
| ICT / QT Alignment (single toggle) | Original | Sam |
| SMT (multi-rank correlated-pair divergence, auto-mapping toggle) | Third-party, embedded | Gregorius_XV — MPL-2.0 |
| CISD detection + Fibonacci projections | Original | Sam |
| True Opens (Session / Day / Week / Month / Year) | Third-party, embedded | Daye's Quarterly Theory (open-source) |
| HTF candle preview panel + dashboard | Original | Sam |
| tCISD, Turtle Soup, CRT H/L | Removed | — |

---

## SMT

The embedded module is Gregorius_XV's `GXV SMT_Divergences[LITE✦]` v2.1.0,
used under MPL-2.0. Two things were adapted on top of it for this build,
both additive — the underlying divergence engine itself is untouched:

- **SMT Auto Mapping** (default **ON**): when on, SMT shows only the single
  rank that matches the CRT engine's active ICT/QT-mapped timeframe (this
  includes an internal 3H rank, used for the ICT 30m → 3H case, since the
  original rank list didn't expose one). When **off**, it falls back to the
  full original behavior — every rank (15m/30m/1H/90m/4H/6H/7H/12H/1D/1W/
  1M/3M/12M) shown or hidden by its own manual toggle, exactly like the
  unmodified GXV script.
- **SMT Text** color defaults to **black** (the original defaults to gray).

## True Opens

Ported in as a namespaced block (`crtTo_` prefix) rather than a separate
indicator. Behavior is unchanged from Daye's source script:

- **One live line + label per type.** A new period's open replaces the
  previous line for that type — it does not accumulate history.
- Lines extend live to the current bar every bar; labels sit to the right by
  `Label Gap` bars.
- Fixed `America/New_York` timezone (matches the source script; not tied to
  the CRT engine's own timezone switch).
- Types: **Session** (Asia 19:30, London 01:30, NY AM 07:30, NY PM 13:30 —
  off by default), **Day** (00:00 NY), **Week** (Mon 18:00 NY), **Month**
  (2nd Monday 18:00 NY), **Year** (April 1, 00:00 NY — Q2 of the yearly
  cycle).
- No 90-minute Open (the source script doesn't have one).
- All five types default to **black**; each has its own color input.
- Labels default to `size.small`.

## Dashboard / HTF panel

Top-right compact table: **CRT Model (timeframe)**, **QT Alignment** mode,
a **True Open Day/Week bias** row, **Bias**, **SMT** pair, **Date**.

- The timeframe shown (both in the table header and in the "TF: … [ICT/QT]"
  label on the HTF candle panel) is formatted for readability — e.g. `6H`,
  `90m`, `1D` — instead of Pine's raw internal string (`360`, `90`, `D`).
  This is display-only; the raw string still drives the actual
  timeframe-mapping logic.
- The bias row compares price against **True Day Open** on intraday chart
  timeframes, or **True Week Open** once the chart is Daily/Weekly/Monthly
  (the day cycle stops being a meaningful reference at that zoom), showing
  `Above TDO` / `Below TDO` (or `TWO`), colored green/red.

The HTF candle preview panel (the small candle stack to the right of price)
draws directly from the same aggregated HTF OHLC arrays the CRT engine
itself maintains — there's no separate/duplicate data path for it.

## CISD

Always active. The earlier `tCISD Mode` toggle that could hide it has been
removed, so CISD detection and its Fibonacci projections run
unconditionally per their own inputs.

---

## What was removed

**tCISD, Turtle Soup, and CRT H/L are gone.** These were a from-scratch
build (never a faithful port of any single source), and have been fully
removed at the user's request — every input, alert, and drawing call tied
to them, plus now-orphaned helper variables that existed only to feed them.
Nothing else was touched by their removal.

---

## Compiling

Pine v6. Single `indicator()` declaration. Compile in TradingView's Pine
Editor and check for errors before relying on it live — this file has been
reviewed line-by-line but not run against live/replay data.
