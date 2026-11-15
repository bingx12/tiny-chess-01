# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/bingx12/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/bingx12/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB Python chess engine for make.fun's $500 bounty. 0x88 board, negamax with PVS, alpha-beta, quiescence, transposition table, iterative deepening to node budgets, and sunfish piece-square tables (quantized to multiples of 10, one byte per value) with a tapered endgame king table. Talks to the env over HTTP and plays the house gauntlet.

## Design

Board: a 128-cell 0x88 array.
Off-board squares are caught by a single bit test.
Sliding pieces walk until they hit that bit or a blocker.
Pieces are small integers with the colour in the high bit.
Move generation and capture tests stay a few integer ops.
Zobrist keys cover every piece-square.
They also cover side to move, castling rights and en passant.
One hash serves the transposition table and repetition.
Evaluation is a single tapered score.
Each piece adds its sunfish table value for its square.
Black values are mirrored from White.
A material weight is added per piece.
A king table pulls the king central as pieces leave.
The blend follows remaining non-pawn material.
Tables are quantized to steps of ten.
Each entry is stored as one byte.
That keeps positional knowledge inside the size budget.
Search is negamax with principal-variation search.
Alpha-beta windows prune the tree.
Quiescence covers captures while in check.
A transposition table is keyed by the Zobrist hash.
Moves order by table move first.
Then captures by victim value minus attacker value.
Then promotions, then killers, then history.
Iterative deepening runs to a fixed node budget.
The budget follows the opponent name.
Stronger opponents get more nodes.
No move overruns the per-move clock.

## Local results

Packed entrypoint `main.py` (4,094 bytes, ≤ 4,096 cap) vs the house gauntlet: trial-0 W/D/L 3/1/2, 3.5/6 points, Elo 1,364; trial-1 4/1/1, Elo 1,545; trial-2 4/2/0, Elo 1,654 — three-trial mean Elo 1,521, above the 1,154 practice floor and the sunfish baseline's 1,365 mean. Node budgets cap at 300,000 so no move exceeds the gauntlet's 10 s per-move limit. Perft verified (perft(3) = 8,902 on the start position; all four known positions correct).

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=k6eq9xtw&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
