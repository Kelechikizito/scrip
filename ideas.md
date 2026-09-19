1. Using curves and Seun Lanlege strategies Order book for quant trading — 44/60 · Viable

A fully onchain central limit order book built for algorithmic and market-making flow rather than retail clicking.

Criterion Score Reasoning
C1 Sponsor tech is the hero ×3 4 Remove Monad and this does not exist — per-block cancel/replace and cheap high-frequency order churn are exactly what 0.4s blocks and parallel execution unlock. Docked one point because Kuru already ships an onchain CLOB on Monad and is a sponsor, so you are not pushing the tech anywhere they haven't.
C2 Demo-ability ×2 4 A live ladder updating every block with bots hitting it is kinetic and legible in 15 seconds. Put a latency counter on screen.
C3 Earliness ×2 3 Only became practical post-Monad-mainnet, but Hyperliquid normalised the onchain order book and Kuru already shipped one here.
C4 Theme alignment ×2 5 "Fully onchain order books that do not need an offchain matching engine" is the track's own headline example. You cannot be more on-theme than this.
C5 Domain knowledge ×2 3 Solidity architecture is your strength and matching-engine design is adjacent. But the track's stated best fit is teams who have shipped a trading or market-making product, and quant/microstructure knowledge is not on your record.
C6 Non-genericness ×1 2 This is the single most predictable Metropolis submission. The track listed it as an example; assume dozens of teams arrive with it.

Strongest criterion: C1 — this is the only idea in the set where the chain is genuinely the reason the product can exist. Weakest criterion: C6 — you will be in a pile, and the pile includes the sponsor's own live product.

What would raise this score:

Pick a mechanism Kuru has not built rather than a better CLOB. Per-block sealed batch auction with uniform clearing price is the obvious one: it kills the latency race and MEV extraction instead of winning it, and it only makes sense on a chain with sub-second finality.
Ship a maker-side SDK and two reference bots, so the demo is bots trading against each other rather than you clicking.
Go after Perpl's $3k analytics/risk bounty with the fill-quality dashboard you'll need anyway. That's one sponsor added with real value, not stacking.

One-line pitch: "An onchain order book that clears in per-block batch auctions — same price for everyone in the block, so there's nothing left to front-run."

2. 1. Private order book using curves for RWA, equities and tokenized stocks — 41 → 49/60 · Build this

Originally scored with "mathematical curves" undefined and Privy named as the privacy layer. With ArcBook's curve primitive substituted in and Privy dropped, this becomes the strongest idea in the set.

Criterion Was Now Reasoning
C1 Sponsor tech is the hero ×3 3 4 Originally 4 minus a sponsor-stacking penalty: Privy is an embedded-wallet and auth SDK, not a privacy technology — "private… built with Privy" had no mechanism behind it. Dropping Privy removes the penalty, and because Aqua does not port, you must build settlement natively on Monad, which turns borrowed plumbing into your own Monad-specific work. Not 5: the curve math itself is chain-agnostic — it ran on Base Sepolia.
C2 Demo-ability ×2 2 4 The biggest single gain in this document. The original had two failure modes — an unexplainable pitch and an invisible payoff. Curves solve both. "Mathematical curves" now means something specific, and the position is a shape on screen: a maker drags alpha, the curve bends, the fill walks along it. You finally have something to show for a mechanism whose other half is invisible by construction.
C3 Earliness ×2 3 3 No change. Curve-native books are genuinely new, but they were publicly demonstrated and awarded eight weeks ago. You are second to a shift, not ahead of it.
C4 Theme alignment ×2 5 5 Unchanged. "Fully onchain order books that do not need an offchain matching engine" is Track 01's own headline example.
C5 Domain knowledge ×2 5 5 Unchanged, with a new risk. Best fit in the set by a distance — UMBRA was a ZK dark pool with compliance, the Arbitrum build was RWA/capital markets, ZK is on your research list. But fixed-point curve kernels with power branches and singular limits are hard numerical Solidity; ArcBook needed an independent high-precision Python reference model to validate theirs. You have the Python and EVM-internals background to do that — budget for it.
C6 Non-genericness ×1 2 3 Curve-native + hidden orders + RWA is a combination nobody has shipped — ArcBook has no privacy layer. Scored 3 not 5 because half of it is borrowed; it collapses to 1 with any judge who recognises the source.

Strongest criterion: C5 — nobody else at this hackathon walks in with a finished ZK dark pool behind them. Weakest criterion: C6 — the novelty is half-borrowed, and borrowed from something eight weeks old and decorated.

Still to fix:

Drop Privy or re-role it. Keep it only as passkey onboarding for the institutional demo account, and stop calling it the privacy layer. Monad's own $2.5k Mera passkey bounties cover the same ground with a sponsor that is actually the host.
Cut RWA + equities + tokenized stocks down to one asset class. Pre-IPO equity is the sharpest, and choosing it absorbs idea 2 entirely.
Make privacy visible even with the curve on screen. Split-screen: left pane is the public mempool view a front-runner sees (noise), right pane is the real curve. Show size filling with no slippage, then reveal the ZK settlement proof verifying.
Check the rules. Submissions must be new work built during the window. You can bring UMBRA's context and team, not its code.

One-line pitch: "Makers publish a curve instead of a ladder — so an illiquid asset has a live two-sided market even when nobody is sitting on the other side, and nobody can see the size coming."
