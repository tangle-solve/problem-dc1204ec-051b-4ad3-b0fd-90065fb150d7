<!-- solve-problem:dc1204ec-051b-4ad3-b0fd-90065fb150d7 -->
# Shared-discount EIP-1559 conjecture

A [Tangle Solve problem](https://solve.tangle.tools/problems/dc1204ec-051b-4ad3-b0fd-90065fb150d7).

## What is the problem?

EIP-1559 is Ethereum's transaction-fee mechanism: users submit bids, the protocol posts a price (base fee), and a user whose bid meets the price is served, paying roughly the posted price. The known theory analyzes *patient* bidders whose value for inclusion never changes. This conjecture concerns what happens when **all users share a common discount rate** δ: a user with value v for immediate inclusion values inclusion one block later at δv, two blocks later at δ²v, etc. The question: consider a modification of EIP-1559 (a "shared-discount" variant, i.e., the mechanism's price trajectory somehow accounts for the common δ) and ask whether it achieves welfare within a constant factor of the optimal offline (or online-clairvoyant) schedule. Formally: for a sequence of users arriving over time, each with (private) value decaying geometrically at rate δ, and a per-block capacity of k transactions, compare the welfare (sum of inclusion-time-discounted values of served users) of the mechanism M_δ against OPT. **Conjecture:** M_δ achieves welfare ≥ c · OPT for some absolute constant c (perhaps approaching 1 as capacity slack grows). **Secondary goal:** quantify the welfare loss as a function of capacity slack (e.g., capacity k vs. demand at most (1+ε)k per block), aiming for 1−O(ε) or similar. The problem statement is informal in the census record; the precise mechanism M_δ and the benchmark (clairvoyant offline vs. online optimal) must be pinned down from the cited Babaioff–Nisan line of work — note this ambiguity.

## Why does it matter?

EIP-1559 is the production fee mechanism of the second-largest blockchain; any statement about its welfare under realistic (impatient, time-sensitive) users directly informs whether it can be trusted not to need redesign. Establishing a shared-discount guarantee extends truthfulness/welfare results from an idealized patient-bidder model to the practically relevant regime where transaction value decays (arbitrage, MEV-sensitive, deadline transactions). If the conjecture fails, it identifies a concrete attack/collusion vector and motivates a concrete patch; if it holds with a slack-loss tradeoff, it gives parameter guidance for block sizing.

## What's already known?

Per the census record: (1) with fully patient bidders, welfare guarantees for EIP-1559-style mechanisms are established; (2) with *heterogeneous* discounts (each user has their own δᵢ), impossibility results exist — no EIP-1559-like mechanism achieves near-optimal welfare. The open case is the interpolation: a single common δ. The key work is Babaioff and Nisan's patient-bidder analysis (arXiv:2502.20031), which establishes the baseline guarantees the conjecture would extend. Related literature on discounted packet scheduling and resource augmentation (competitive analysis with speed/buffer augmentation, e.g., the online scheduling tradition of Kalyanasundaram–Pruhs) provides technical machinery: geometric value decay is exactly the model in much of that literature. I could not verify the exact contents of the 2025–2026 papers beyond the census summary (verify).

## What would count as solving it?

- **Prove:** for the shared-discount EIP-1559 variant, welfare(M_δ) ≥ c·OPT for an explicit constant c (c = 1 would be ideal), under the standard Bayesian/adversarial arrival model of the patient paper.
- **Or refute:** construct an arrival sequence and common δ where any EIP-1559-like mechanism loses ω(1) factor (constant arbitrarily bad), separating common-δ from patient case.
- **Quantify slack:** prove welfare ≥ (1−f(ε))·OPT when capacity exceeds peak demand by factor (1+ε), with explicit f; or show a matching lower bound.
- A referee needs: precise mechanism definition, precise benchmark, complete proof or explicit counterexample.

## How to start

1. Read arXiv:2502.20031 (Babaioff–Nisan) carefully: extract the exact arrival model, the definition of the EIP-1559 abstraction, and the proof technique for the patient case; identify where patience is used.
2. Read the follow-up (2025–2026, per census) for the heterogeneous-discount impossibility; understand *why* heterogeneity breaks it — the natural guess is the common case breaks only via price-update dynamics, which may be repairable.
3. Connect to discounted-packet scheduling: geometrically decaying values are the classic model; known greedy/competitive algorithms (highest-discounted-value-first) may be implementable as an EIP-1559 price rule.
4. Main obstacle: EIP-1559's base fee is user-anonymous and demand-based, so it cannot see individual values; the analysis must show price dynamics alone sort by discounted value well enough.

## Key references

- Babaioff, M., and Nisan, N. (2025). *On the Welfare of EIP-1559 with Patient Bidders*. arXiv:2502.20031. Establishes welfare/truthfulness guarantees for EIP-1559-style mechanisms when bidders are patient; the baseline this conjecture extends. https://arxiv.org/abs/2502.20031
- Babaioff and Nisan (2025–2026), follow-up on impatient/discounted bidders (per census; exact citation unverified — verify). Contains the heterogeneous-discount impossibility motivating the common-δ open case.
- (Background, verify relevance) Kalyanasundaram & Pruhs, and the discounted-objective online scheduling literature, for competitive-analysis and resource-augmentation techniques with geometrically decaying values.

## Warnings

- The census record does not define the "shared-discount EIP-1559" mechanism precisely; your first task is to write down the exact mechanism (how the base fee interacts with δ) — a wrong formalization invalidates everything after.
- Ambiguity in the benchmark (clairvoyant offline vs. online optimal) changes the answer; state it.
- Don't assume patience can be "simulated" by discounting values to a common time — timing of inclusion changes payoffs, not just magnitudes.
- The heterogeneous impossibility likely does not automatically extend to common δ; check whether its counterexample uses disparate δ values essentially.
- The March 2026 date and follow-up paper details come from a census record, not verified sources.

---
*`BCGT-000065` · unverified*

## Acceptance guidance

- **Prove:** for the shared-discount EIP-1559 variant, welfare(M_δ) ≥ c·OPT for an explicit constant c (c = 1 would be ideal), under the standard Bayesian/adversarial arrival model of the patient paper.
- **Or refute:** construct an arrival sequence and common δ where any EIP-1559-like mechanism loses ω(1) factor (constant arbitrarily bad), separating common-δ from patient case.
- **Quantify slack:** prove welfare ≥ (1−f(ε))·OPT when capacity exceeds peak demand by factor (1+ε), with explicit f; or show a matching lower bound.
- A referee needs: precise mechanism definition, precise benchmark, complete proof or explicit counterexample.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/dc1204ec-051b-4ad3-b0fd-90065fb150d7?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000065.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000065)
