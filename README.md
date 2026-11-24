# Tiny Chess Engine
[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zelucian/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zelucian/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A compact, deterministic chess engine that lives in a single
`main.py`, written for the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01).

## How it plays

The bot talks to the make.fun environment over the usual `/game` and
`/move` endpoints and thinks in nodes, not seconds: every move gets
the same fixed budget of **120,000 searched nodes**, and when the
budget runs out mid-iteration the engine simply keeps the best move
from the last iteration it managed to finish.

Inside, it is a fairly classical design squeezed hard for size:

- a 0x88 board with fully legal move generation — castling, en
  passant and promotions included — checked against published perft
  counts
- negamax alpha-beta with principal-variation search and a
  transposition table keyed by an incremental Zobrist hash (piece
  squares, castling rights, side to move, en-passant file)
- a quiescence search with stand-pat, capture and promotion lines,
  and a full evasion search whenever the side to move is in check
- move ordering that tries the table move first, then captures by
  MVV-LVA, then promotions
- evaluation from material plus piece-square tables and a small
  tempo bonus, maintained incrementally as moves are made and unmade
- draw awareness: positions repeated along the search path and the
  fifty-move counter are scored with a contempt of 25 centipawns

## The 4 KB trick

`main.py` is not the file I edit. The readable source is developed
separately and then packed with the competition kit's own
`makefun compress` command, which emits the LZMA-packed form the
rules expressly allow, comfortably inside the 4 KB limit.

## Checking it

The test suite exercises make/unmake state integrity over random
games, hash exactness against a from-scratch hash after every move,
perft on the start position, Kiwipete and the CPW test positions,
mate-in-1, determinism (same position, same move, every time) and a
worst-case move-time check at the full production budget.

## License

MIT — see `LICENSE`.
