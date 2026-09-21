# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Hackathon entry for Monad: an on-chain order book. The product direction is **not decided yet** — don't assume one. Context lives in:
- `ideas.md` — candidate ideas with rubric scores (batch-auction CLOB vs. private curve-based order book)
- `execution.md` — background on order books, quant trading, and the solver/RFQ alternative
- `tracks-eligibility.md` — sponsor bounty requirements (sponsor choices also undecided)
- `inspo/` — reference projects behind the ideas (e.g. `inspo/arcbook.md`). How scrip borrows from them is undecided; read on demand, don't treat as spec.
- `.claude/rules/monad-differences.md` — Monad vs. Ethereum behavior (loaded automatically)

`src/Counter.sol`, `test/Counter.t.sol`, `script/Counter.s.sol` are Foundry scaffolding placeholders — replace them, don't build on them.

## Build / check

Foundry project. CI (`.github/workflows/test.yml`) runs, in order:

```bash
forge fmt --check
forge build --sizes
forge test -vvv
```

A change isn't done until all three pass. Run a single test with `forge test --mt <testName> -vvv`.

## Conventions

- New contracts follow the user's `/sol-style-guide` skill: its file layout, section ordering, and full NatSpec.
- `lib/forge-std` and `lib/openzeppelin-contracts` are git submodules — never edit them. After cloning, run `git submodule update --init --recursive`.
