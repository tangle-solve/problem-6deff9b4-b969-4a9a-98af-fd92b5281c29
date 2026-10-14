<!-- solve-problem:6deff9b4-b969-4a9a-98af-fd92b5281c29 -->
# Stable strategic interest-rate controllers

A [Tangle Solve problem](https://solve.tangle.tools/problems/6deff9b4-b969-4a9a-98af-fd92b5281c29).

## What is the problem?

In automated lending markets (e.g., Compound/Aave-style "protocols for loanable funds"), the interest rate offered in each pool is set by a deterministic controller, typically a piecewise-linear (often piecewise-exponential) function of **utilization** U_t = Borrows_t / (Borrows_t + Liquidity_t). Agents — borrowers and lenders — receive utility depending on rates and can **strategically migrate** their capital among multiple pools to arbitrage rate differences. Agents act on **delayed/stale observations** of pool state (block latency, update granularity).

Formalize as a dynamical game: state x_t = (B^1_t, ..., B^n_t, L^1_t, ..., L^n_t) for n pools; each pool i posts rate r^i_t = f_i(U^i_t); a continuum (or finite set) of agents chooses a flow of reallocation m_t based on information available at time t − τ (delay τ ≥ 1); the state updates from flows. The open question: **under what conditions on the controllers {f_i} and the information/latency structure does the induced closed-loop dynamics have a (stable) equilibrium — e.g., convergence, bounded oscillation amplitude, or existence of a rest point — and what welfare (aggregate interest/liquidity) does that equilibrium achieve?** "Stability" here most reasonably means asymptotic or bounded-oscillation stability of utilization around a target band; the census leaves both the stability notion and the agent model (best-response vs. learning dynamics) informal, so a solver must fix these explicitly.

## Why does it matter?

Runaway utilization feedback ("liquidity spirals") is a documented systemic risk: rate spikes trigger withdrawals, which spike rates further, forcing liquidations and amplifying stress across composable DeFi protocols. A provable stability/welfare guarantee would let protocol designers certify controller parameters (slope, kink placement) before deployment, and would underpin safety analyses of cross-protocol credit chains. Conversely, an impossibility result (controllers that cannot be stabilized under migration + delay) would justify redesign toward explicit feedback control or admission of instability costs.

## What's already known?

Gudgeon et al. (2020) introduced the loanable-funds model, showed DeFi rates deviate from ideal-market benchmarks, and flagged rate-controller design as a problem — but treated agents as non-strategic price-takers with no delay. Empirical/follow-up work has measured rate stickiness and pool migrations qualitatively. The classical analogy is the **cobweb model**: delayed supply response to price causes oscillation when demand/supply slopes are misaligned; the natural conjecture is that piecewise-linear controllers with steep post-kink slopes are cobweb-unstable once agents best-respond with delay. No published result, to our knowledge, proves stability or instability of the multi-pool migration game (verify). Control-theoretic analyses of AMMs and stablecoins exist but do not cover strategic lender migration with delay.

## What would count as solving it?

1. **Stability theorem**: For specified controller class f_i and agent best-response (or no-regret learning) with delay τ, prove utilization converges to a neighborhood (give the radius) or derive a tight sufficient condition (e.g., slope × migration elasticity × delay < 1).
2. **Impossibility/lower bound**: Construct agent strategies forcing divergence or oscillations of amplitude Ω(·) for any controller in the deployed class.
3. **Welfare characterization**: At the stable equilibrium, bound the welfare gap vs. the ideal loanable-funds benchmark of Gudgeon et al.
4. Optionally: a validated simulation/experiment on forked mainnet data demonstrating predicted oscillation thresholds.

## How to start

1. Read Gudgeon et al. (2020), Sections on the interest-rate model and market efficiency comparison, to fix the baseline pool model.
2. Reduce to the **single-agent, two-pool, zero-delay** case: show best-response migration against piecewise-linear utilization curves is a cobweb-style map; characterize when it's a contraction. This is the core lemma.
3. Add delay τ: use the classical cobweb stability criterion for linearized delayed feedback (roots of z^τ = a·slope condition) to get a sufficient condition, then attempt extension to n pools via monotone dynamical systems or diagonal dominance.
4. Main obstacle: strategic agents make the "demand curve" endogenous — the migration elasticity depends on beliefs, so you'll likely need to assume a revealed-preference/elasticity bound or model agents as gradient/no-regret dynamics to close the loop.

## Key references

- Gudgeon, L., Perez, D., Werner, S., & Knottenbelt, W. J. (2020). *DeFi Protocols for Loanable Funds: Interest Rates, Liquidity and Market Efficiency*. arXiv:2006.13922. Defines the loanable-funds model for Compound-style pools, analyzes interest-rate dynamics, and shows deviations from efficient-market benchmarks. https://arxiv.org/abs/2006.13922
- (Background, verify) Classical cobweb literature, e.g., Ezekiel (1938), "The Cobweb Theorem" — the delayed supply-response oscillation model this problem structurally generalizes.

## Warnings

- The census record is informal: no precise agent model, delay structure, or stability definition is fixed. State yours explicitly and justify.
- Do not assume instantaneous utilization observation — DeFi rates update per block/tx and agents sample asynchronously; symmetric synchronous delay is the tractable starting case.
- Beware conflating "controller stability" (utilization dynamics) with "market equilibrium stability" (agent migration); both matter but require different proofs.
- A common dead end: analyzing a single pool in isolation — instability here is inherently multi-pool, driven by arbitrage flows.

---
*`BCGT-000342` · flagged for review*

## Acceptance guidance

1. **Stability theorem**: For specified controller class f_i and agent best-response (or no-regret learning) with delay τ, prove utilization converges to a neighborhood (give the radius) or derive a tight sufficient condition (e.g., slope × migration elasticity × delay < 1).
2. **Impossibility/lower bound**: Construct agent strategies forcing divergence or oscillations of amplitude Ω(·) for any controller in the deployed class.
3. **Welfare characterization**: At the stable equilibrium, bound the welfare gap vs. the ideal loanable-funds benchmark of Gudgeon et al.
4. Optionally: a validated simulation/experiment on forked mainnet data demonstrating predicted oscillation thresholds.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/6deff9b4-b969-4a9a-98af-fd92b5281c29?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000342.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000342)
