# Inspiration: ArcBook

- Live demo: https://arcbook-nu.vercel.app/#/home
- Repo: https://github.com/Ryad2/liquid_OB (default branch `main`)
- Saved: 2026-09-21. The README below is copied as-is, with relative links rewritten to point into that repo.
- **How scrip takes inspiration from this is NOT decided yet.** Don't treat it as a spec or copy its design without asking. `ideas.md` idea 2 already relies on ArcBook's curve primitive.
- ArcBook's own code is AGPL-3.0-only, and Aqua/SwapVM carry separate Degensoft terms. Borrowing ideas is fine; copying code would bring those licenses into this repo.

---

# ArcBook

ArcBook is a functional order book in which makers publish bounded, executable
pricing curves instead of only flat price-and-size orders.

## Status

This repository was initialized from an empty GitHub repository on 25 July
2026 during ETHGlobal Lisbon. The onchain hackathon MVP now includes canonical
curve encoding and math, two-sided recycled runtime, official Aqua/SwapVM
settlement, lifecycle Lens, product Quoter, bounded atomic multi-maker routes,
adversarial security tests, deployment/seed/replay scripts, generated ABIs, a
typed position SDK, deterministic multi-maker solver, native Subgraph and
stateless solver API. ArcBook now has both a deterministic mock and a
fail-closed live adapter with wallet-driven publish, execute and dock flows.
A read-only Executable Liquidity MCP, release containers, deployment workflows,
health/metrics endpoints and a zero-localhost release gate are also implemented.
The public demo is deployed on Base Sepolia, indexed by The Graph, and served
through one HTTPS Vercel application with live solver and MCP functions.

## Live Demo

- Application: <https://arcbook-nu.vercel.app>
- Solver API: <https://arcbook-nu.vercel.app/api/solver>
- Executable Liquidity MCP: <https://arcbook-nu.vercel.app/api/mcp>
- Subgraph: <https://api.studio.thegraph.com/query/1757012/arcbook/v0.1.0>
- Immutable deployment manifest: [`deployments/84532.json`](https://github.com/Ryad2/liquid_OB/blob/main/deployments/84532.json)
- Network: Base Sepolia (`84532`)

The seeded release contains one dETH/dUSD market, three independently
parameterized maker positions and indexed multi-maker route executions. Run
`pnpm release:verify` with the public URLs from `docs/DEPLOYMENT.md` to verify
the complete zero-localhost topology.

Protocol work was introduced through small, reviewable commits. External tools
and dependencies are recorded as they are introduced.

The current execution plan is documented in
[`docs/HACKATHON_PLAN.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/HACKATHON_PLAN.md).

The complete end-to-end implementation architecture and protocol-integration
map is documented in [`docs/ARCHITECTURE.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/ARCHITECTURE.md).

The dependency-ordered development sequence, exit gates, tests, and intended
commit history are documented in
[`docs/IMPLEMENTATION_ORDER.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/IMPLEMENTATION_ORDER.md).

The normative units, identifiers, route ABI, event contract, byte offsets, and
version 1 test vector are documented in
[`docs/WIRE_FORMAT.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/WIRE_FORMAT.md).

The page-by-page ETHGlobal rules audit, public-demo topology, and mandatory
zero-localhost submission gate are documented in
[`docs/ETHGLOBAL_RULES_COMPLIANCE.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/ETHGLOBAL_RULES_COMPLIANCE.md).

The decision to keep Uniswap v4 custom accounting as a post-MVP alternative,
rather than the hackathon settlement core, is documented in
[`docs/UNISWAP_V4_HOOK_EVALUATION.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/UNISWAP_V4_HOOK_EVALUATION.md).

The complete product and protocol specification is documented in
[`docs/PRODUCT_SPEC.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/PRODUCT_SPEC.md).

The normative fixed-point and exchange-rate model is documented in
[`docs/MATH_SPEC.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/MATH_SPEC.md).

The independent derivation and validation record is documented in
[`docs/MATH_AUDIT.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/MATH_AUDIT.md).

The pinned transcendental backend, numerical domains, approximation intervals,
and conditioning limits are documented in
[`docs/TRANSCENDENTAL_MATH_AUDIT.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/TRANSCENDENTAL_MATH_AUDIT.md).

The independent `Decimal` oracle, deterministic vector schema, regeneration
procedure, and trust boundary are documented in
[`docs/REFERENCE_MODEL.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/REFERENCE_MODEL.md).

The exact implemented/missing capability inventory is documented in
[`docs/IMPLEMENTATION_STATUS.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/IMPLEMENTATION_STATUS.md). Frontend
developers should use [`docs/FRONTEND_HANDOFF.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/FRONTEND_HANDOFF.md)
and [`packages/frontend-api/`](https://github.com/Ryad2/liquid_OB/blob/main/packages/frontend-api/README.md) as their entry
point.

The finalized Uniswap developer feedback document is available in
[`FEEDBACK.md`](https://github.com/Ryad2/liquid_OB/blob/main/FEEDBACK.md).

## Uniswap Submission Verification

ArcBook is submitted as a new onchain market primitive under a written sponsor
exception that does not require an artificial Uniswap API call. The exception
is retained privately; this repository does not claim an API integration that
it did not implement.

The code paths submitted for evaluation are linked directly below:

- [`LiquidOBCurveKernel.quotePosition`](https://github.com/Ryad2/liquid_OB/blob/main/contracts/src/core/LiquidOBCurveKernel.sol#L53-L115)
  compiles and quotes one executable maker curve before applying its two-sided
  state transition.
- [`LiquidCurveInstruction._liquidCurve`](https://github.com/Ryad2/liquid_OB/blob/main/contracts/src/core/LiquidCurveInstruction.sol#L49-L76)
  materializes a quoted fill and commits the next runtime state.
- [`LiquidOBBatchExecutor.executeExactInput`](https://github.com/Ryad2/liquid_OB/blob/main/contracts/src/periphery/LiquidOBBatchExecutor.sol#L46-L119)
  atomically settles a solver-selected route across multiple maker positions.
- [`FEEDBACK.md`](https://github.com/Ryad2/liquid_OB/blob/main/FEEDBACK.md) records the documentation reviewed, the approved
  integration scope, concrete friction, and suggested improvements.

## Workspace

- `contracts/`: Foundry workspace for EVM contracts and tests.
- `apps/web/`: React and TypeScript demo application.
- `packages/`: Shared TypeScript packages and generated clients.
- `packages/contracts/`: generated ABIs and validated deployment manifests.
- `packages/position-sdk/`: publish, quote, Lens, execute and dock helpers.
- `packages/frontend-api/`: stable UI contract, amount helpers, and mock client.
- `packages/frontend-live/`: manifest-validated live product and transaction adapter.
- `packages/solver-core/`: deterministic exact-input/output multi-maker solver.
- `subgraph/`: native protocol indexing, mappings, queries and Matchstick tests.
- `services/solver-api/`: Graph-to-Lens-to-Quoter route orchestration service.
- `services/liquidity-mcp/`: reusable Graph-backed executable-liquidity MCP service.
- `tools/reference/`: development-only high-precision mathematical oracle.
- `test/vectors/`: committed language-neutral protocol vectors.
- `docs/`: design decisions, provenance, security notes, and integration logs.
- `prompts/`: material AI-assisted specifications and implementation plans.

## Prerequisites

- Node.js 24.18.0
- pnpm 10.32.1
- Foundry 1.5.1
- Solidity 0.8.30 (installed automatically by Foundry)
- Python 3.10 or newer (standard library only)

With `asdf` installed, run:

```bash
git submodule update --init --recursive
asdf install
pnpm install --frozen-lockfile
```

## Checks

```bash
pnpm check
pnpm lint
pnpm build
pnpm test
pnpm contracts:fmt
pnpm contracts:lint
pnpm contracts:build
pnpm contracts:test
pnpm reference:test
pnpm reference:check
```

The optional official Base fork proof requires `BASE_MAINNET_RPC_URL`; without
it, that suite is reported as skipped. See
[`docs/DEPENDENCY_AUDIT.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/DEPENDENCY_AUDIT.md) for the exact command and
verified deployment matrix.

The reproducible public deployment and seeded-demo flow is documented in
[`docs/DEPLOYMENT.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/DEPLOYMENT.md).
The same runbook includes the supported single-project Vercel topology for the
ArcBook frontend, solver API and read-only MCP endpoint.

The exact three-minute submission recording sequence, narration, and fallback
capture plan are documented in
[`docs/DEMO_VIDEO_SCRIPT.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/DEMO_VIDEO_SCRIPT.md).

The Base Sepolia default, EVM portability boundary and Aqua/SwapVM deployment
conditions are documented in
[`docs/CHAIN_SELECTION.md`](https://github.com/Ryad2/liquid_OB/blob/main/docs/CHAIN_SELECTION.md).

`pnpm release:verify` is the final public gate. It rejects localhost/private
URLs and verifies the hosted app, contracts, manifest, Subgraph, solver API,
MCP service, seeded positions and indexed route evidence.

## Security

This is hackathon software and is not production-ready or audited. Do not use
it with assets of value. See `SECURITY.md` before reporting a vulnerability.

## License And Attribution

Independent ArcBook code is licensed under the
[GNU Affero General Public License v3.0 only](https://github.com/Ryad2/liquid_OB/blob/main/LICENSE). Files and submodules
carrying a different SPDX identifier or their own license remain governed by
those terms. Full Aqua and SwapVM terms and notices are preserved in
[`LICENSES/`](https://github.com/Ryad2/liquid_OB/blob/main/LICENSES/README.md).

Powered by Aqua — © Degensoft Ltd 2025

Powered by SwapVM — © Degensoft Ltd 2025

These are factual integration attributions and do not imply endorsement.
