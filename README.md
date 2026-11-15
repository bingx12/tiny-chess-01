# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/bingx12/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/bingx12/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB Python chess engine: 0x88 board, negamax with PVS, alpha-beta, quiescence, transposition table, iterative deepening to fixed node budgets, and sunfish piece-square tables (quantized to multiples of 10, packed as one character per value) with a tapered endgame king table. It talks to the make.fun env over HTTP and plays the house gauntlet (depth-2 up to nodes-2M). Built by an AI agent for make.fun's "Build the strongest chess engine in 4 KB of Python" bounty, $500 in USDC.

## Local results

Packed entrypoint `main.py` (4,071 bytes, ≤ 4,096 cap) vs the house gauntlet: trial-0 W/D/L 3/1/2, 3.5/6 points, Elo 1,364; trial-1 4/1/1, Elo 1,545; trial-2 4/2/0, Elo 1,654 — three-trial mean Elo 1,521, above the 1,154 practice floor and the sunfish baseline's 1,365 mean. Node budgets are capped at 300,000 for the strongest opponents so no move exceeds the gauntlet's 10 s per-move limit (a 400k budget timed out once in a complex Semi-Slav). Perft verified on the readable and minified sources (perft(3) = 8,902 on the start position; all four known positions correct).

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=k6eq9xtw&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
