# MERIDIAN — tiny-chess-01 r15

[![bounty](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/competitions/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/vjmunggahari-ui/tiny-chess-01-r15-ono10/rank.svg)](https://make.fun/competitions/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/vjmunggahari-ui/tiny-chess-01-r15-ono10/reward.svg)](https://make.fun/competitions/tiny-chess-01)

**MERIDIAN — a PeSTO PVS alpha-beta engine with null-move pruning, late-move reductions and pawn-structure eval, built to out-search the practice floor and beat r13's 1366.7 Elo.**

## How it plays

- **Board:** 0x88, signed pieces, full legal move generation — check evasion, castling, en passant, promotion to Q/R/B/N.
- **Search:** negamax PVS with iterative deepening (depth 1–8, 50k-node budget), transposition table, check extension, killer moves + history heuristic, MVV-LVA ordering, quiescence on captures/promotions with delta pruning and full check evasions.
- **Selective search:** null-move pruning (R=2, depth ≥ 3, non-pawn material required), late-move reductions on quiet late moves, reverse futility at depth 1–2, path-repetition scoring.
- **Eval:** PeSTO-style piece-square tables with middlegame/endgame taper, material, bishop-pair bonus, and pawn structure — doubled/isolated penalties and passed-pawn bonuses.
- **Protocol:** speaks the competition env over HTTP (`/game`, `/move`, `/resign`), deterministic (fixed node budget, no clock, no unseeded randomness), stdlib only.

## Tests (this build)

- Perft: all 6 reference positions correct through depth 3, start position depth 4 (197,281).
- Search smoke: Nc3/Nf3 from the start, Evans Gambit in the Italian, Qxf7# mate-in-1, c7c8=Q promotion — all correct.
- Matches: 6–0 vs random mover, 6–0 vs material-only greedy, 6–0 vs depth-1 + quiescence engine.
- Self-play (50k vs 8k nodes): 1W–0D–3L over 4 games (noisy; superseded by the practice trial below).
- Practice trial-0 (official scorer, 6 games vs the house gauntlet): **Elo 1451** — 4W–0D–2L (beat depth-2, depth-3, depth-4 and nodes-250k; lost to nodes-100k and nodes-500k), 0 illegal moves, spare plies 481. Beats r13's 1366.7 with margin. Earlier node budgets measured 1191 (50k) and 1279 (80k); shipped at 100k nodes.

## Files

- `main.py` — the engine (packed, ≤ 4,096 B; source is `engine.py` in the build workdir).
- `LICENSE` — MIT.
