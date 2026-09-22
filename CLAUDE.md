# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Hackathon entry for Monad: an on-chain order book. The product direction is **not decided yet** — don't assume one. Context lives in:
- `ideas.md` — candidate ideas with rubric scores (batch-auction CLOB vs. private curve-based order book)
- `execution.md` — background on order books, quant trading, and the solver/RFQ alternative
- `tracks-eligibility.md` — sponsor bounty requirements (sponsor choices also undecided)
- `hackathon-resources.md` — official Metropolis resource catalog (Monad docs, templates, SDKs, MCP servers/skills, sponsor perks, per-idea reading lists). Check it first when you need a Monad doc link, RPC/indexer, or reference for an order-book design (see "Execution-Aware Trading Interfaces", "New Order Types via Better UI", "Continuous Liquidity for Illiquid Assets")
- `inspo/` — reference projects behind the ideas (e.g. `inspo/arcbook.md`). How scrip borrows from them is undecided; read on demand, don't treat as spec.
- `.claude/rules/monad-differences.md` — Monad vs. Ethereum behavior (loaded automatically)
- `monad-docs` MCP server (`.mcp.json`) — search/fetch docs.monad.xyz. Prefer it over memory for Monad-specific behavior

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
- UI sound effects: [cuelume](https://www.npmjs.com/package/cuelume) is installed in `frontend/` but not wired up. When the user asks for interaction sounds, follow its agent guide at https://cuelume-site.pages.dev/agents.md (call `bind()` once from a client component, tag elements with `data-cuelume-*`). Don't wire it until asked.
- `lib/forge-std` and `lib/openzeppelin-contracts` are git submodules — never edit them. After cloning, run `git submodule update --init --recursive`.
