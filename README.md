# Tiny Chess Engine
[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zelucian/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zelucian/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A deterministic chess engine in a single `main.py`, built for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01).

## What it is

`main.py` is a self-contained Python chess bot that speaks the make.fun
environment protocol (`/game`, `/move`) and searches with a fixed budget
of **140,000 nodes per move**, never a clock.

The competition kit's own `makefun compress` command produces `main.py`
from the readable source: an LZMA-packed form expressly allowed by the
rules, within the 4 KB cap.

- 0x88 board, fully legal move generation (perft-verified)
- Negamax alpha-beta with PVS, check extensions, killer-move ordering,
  and a transposition table with mate-score adjustment
- Quiescence search over captures and promotions, with full check
  evasions and mate detection
- Incremental PST evaluation with bishop-pair bonus; Zobrist hashing
- Draw handling: repetition along the search path and the fifty-move
  rule

## Validation

Move generation is verified against known perft counts (start position,
Kiwipete, CPW positions 3–4), make/unmake state integrity, evaluation
symmetry, mate-in-1 and tactical-shot tests, and a determinism check
(same position → same move, always).

## License

MIT — see `LICENSE`.
