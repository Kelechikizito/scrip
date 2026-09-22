**What an orderbook is**

- A list of all the buy offers and sell offers for an asset at any moment. Buyers post the price they're willing to pay; sellers post the price they'll accept.
- The highest buy price is the "bid," the lowest sell price is the "ask." The gap between them is the "spread."
- When a buy order and a sell order match on price, a trade happens and both orders come off the book.
- Orders sit in a queue sorted by price first, then by who arrived first at that price. This ordering rule is called price-time priority.
- Unlike an AMM (where a formula sets the price), an orderbook lets people name their own prices. That's why traditional exchanges like Nasdaq use orderbooks, not formulas.

**What quant trading is**

- Trading driven by math, statistics, and code rather than gut feeling or news reading. A program decides what to buy and sell based on rules derived from data.
- Strategies are usually backtested on historical data before running live, to check whether the edge is real or just luck.
- Common examples: market making (constantly quoting both a buy and sell price and earning the spread), arbitrage (exploiting price differences between venues), and statistical patterns like mean reversion.
- Most quant strategies live or die on execution speed and fees, because the profit per trade is often tiny and they win by doing it many times.

**How the two fit together**

- Quant strategies need an orderbook because they mostly trade by placing and cancelling limit orders, not by taking whatever price a formula gives them. An AMM gives them very little control.
- Market making — the biggest quant strategy — literally _is_ posting orders on both sides of a book. Without an orderbook there's nothing to post to.
- The orderbook is also the main data source: the depth, spread, and order flow are the inputs quant models read to predict where price is heading.
- On-chain, the hard part is that every order placement and cancellation costs gas and takes block time. Quant strategies quote and cancel constantly, so a naive on-chain book gets too expensive to use.
- This is why most on-chain orderbook designs keep matching off-chain (fast, cheap) but settle trades on-chain (trustless). The design question you'll face is exactly where to draw that line.

---

This thread is describing a **solver-based RFQ model dressed as an orderbook**, and it's a useful reference point for what you're building.

**What they actually built**

- Instead of users posting orders, professional market makers ("solvers") post quotes — price and size they're willing to fill at.
- The "book" is just a display of those quotes. No capital is escrowed in it, which is why they call it virtual.
- Solvers keep their funds in their own accounts until a user picks a quote, at which point settlement happens on-chain.
- Multiple solvers competing on the same pair is what creates price discovery — that's the bit they say a single liquidity pool couldn't give them.

**Why they moved off the pool design**

- An AMM pool needs LPs to lock capital in advance, and for FX that means idle capital sitting in pairs nobody is trading at that moment.
- A quote-based book lets solvers price dynamically and only commit capital when a trade actually happens. Much more capital-efficient for long-tail pairs.

**What this means for your build**

- This is a lighter variant of the hybrid design I described: the off-chain part is _quotes from a small set of solvers_ rather than a full order queue from the public.
- It's genuinely easier to ship — you don't need a matching engine, just quote aggregation and an on-chain fill contract that verifies a signed quote and swaps atomically.
- The tradeoff is that it's not a real orderbook. There's no price-time priority, no resting user limit orders, and no depth from retail. Quant strategies that want to _post_ liquidity can't participate — they can only take solver quotes.
- So the design question for you is which side you're serving. If your users are quants who want to run market-making strategies, a solver RFQ model shuts them out and you need a true book with resting orders. If your users are people who just want good execution, the solver model is the faster path.

One flag worth keeping in mind: their claim that "all execution remains onchain" is doing some work. Execution settles on-chain, but price formation happens entirely off-chain among a permissioned-ish set of solvers, which is a meaningfully different trust story than an on-chain book. Worth deciding early which of those two you're actually promising.
