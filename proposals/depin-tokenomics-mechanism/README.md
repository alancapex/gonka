# Proposal: A DePIN-Native Tokenomics Mechanism for GNK

Nov 16, 2026

**Proposer:** Ancapex Team

**Status:** draft v0.4 · pre-proposal for discussion (no on-chain vote requested)

## Summary

We announced an earlier version of this proposal to the community and discussed it in an AMA; it was not put to a vote. Most of that text argued that Gonka's tokenomics needs to change at all. Bitfury's proposal has since made that case, so this version does not repeat it.

Bitfury's proposal sets out to make inference the main source of host income: it moves the price towards market and adds a burn of fees. We thank Bitfury for this work and support it as the initial mechanism.

This proposal funds the design of an alternative mechanism that we consider better suited to a DePIN/DeVIN network.

We read a utility token as the settlement currency of its network, not as a share in it. A share is valued by the profit left for its holders; a currency is valued by the volume of work settled in it and by the rule that governs its supply. On that reading emission is the network's monetary policy, and it should respond to the work the network does, not only to the calendar.

The design also has to start from Gonka's own situation. Inside the network, the mechanism has to work with the escrow through which work is paid for, and extend beyond inference to other kinds of work, such as training. Outside it, Gonka is not a crypto project in a vacuum: it competes with AI inference providers that subsidise their prices, and the calibration has to take those subsidies into account.

## What the mechanism is

The mechanism has three parts:

1. **Dynamic mint + burn.** Part of each payment is burned, and emission is set by a rule tied to the work the network does. How much to burn and how emission follows work are computed, not assumed.
2. **Pricing beyond inference.** One pricing method for every type of work the network sells: inference, training, and what comes next. The GNK/USD rate and price feed are designed with both Bitfury's approach and an alternative in view.
3. **Tested protection against fake work.** Adversarial tests showing that self-dealt load cannot earn more than it costs. The original tokenomics design centred on this protection, and a redesign must not lose it.

One payment and one epoch, step by step:

1. **A dollar price.** Each type of work has a price in dollars. The broker pays it in GNK, at the network's rate, when it creates escrow.
2. **A burn at settlement.** When the work is settled, a share of the payment is burned and the rest goes to the host.
3. **Emission by rule.** Each epoch the network issues GNK in two parts: one for proven capacity, by PoC weight, and one for confirmed paid work. The total stays within the current schedule and the 1B cap.
4. **No mint in response to the burn.** What is burned is not issued back. The burn grows with revenue in dollars while emission is set in tokens, and that ties the price of GNK to the work the network does.

The parts are not new. Helium and Render burn payments and pay providers from a separate emission. io.net, since June 2026, sets supplier pay in dollars and releases only the tokens needed for it. Akash shows what to avoid: its burn-mint issues the token back to the provider at settlement, so a net burn comes only from price moves in between.

None of these is proof of outcome. Helium's HNT fell 77% in 2025 while revenue grew about 8×. The settings decide, which is why this proposal is about computing and testing them for Gonka.

## What we will do and deliverables

**1. Dynamic mint + burn.** We will write two rules: one that burns a share of each payment at settlement, and one that sets per-epoch emission from proven capacity and confirmed paid work, within the current schedule and the 1B cap. We will define their parameters: the share to burn, how closely emission may follow work, the treatment of burned coins, and the transition for hosts paid by PoC weight today.

Deliverable: a formal specification of the burn rule and the emission rule: formulas, parameters and their bounds.

**2. Pricing beyond inference.** We will write a dollar price list for each type of work (inference with input and output priced separately, training, and later classes) and the rule for the GNK/USD rate applied when escrow is created. We will benchmark the price list against market prices, adjusting for the subsidies behind them. For the rate and the price feed we will compare Bitfury's approach with an alternative, including how each can be skewed and by whom.

Deliverable: a specification of the price list and the rate rule, with the two approaches compared.

**3. Tests of protection against fake work.** We will check that the new mechanism keeps the protection against fake work that Gonka's original tokenomics design centred on. We will build adversarial tests in which a host sends itself paid requests, and measure what that load costs and what it earns under the rules from items 1 and 2.

Deliverable: a test suite and a report with the parameter ranges in which fake work loses money.

**4. Network model and stress tests.** We will build an open simulator of hosts, brokers, users, price, emission and burn, and calibrate it on chain data. We will run it through four scenarios: a fall in demand and in price, hosts leaving, rate manipulation, and competitors cutting subsidised prices. The initial mechanism runs on the same scenarios as the baseline.

Deliverable: public code and data, and a report with the parameter ranges in which the network survives.

## Payment Request

We request **50,000 USDT and 200,000 GNK** to fund work on all four items.

## Why This Team

Ancapex is a team with experience in, and a focus on, AI and crypto. We are an early Gonka miner, a contributor to Gonka's payment system, and an active member of the community.

**Conflict of interest.** The authors are active hosts and operate a GNK brokerage. A change in tokenomics affects us in the same way as other hosts and brokers. The variant of each mechanism and all parameter values are decided by separate votes, not by the authors.

## Funded-Work Rules

- **Open source:** specifications in `proposals/` of the main repo; simulator, tests and data in [repo, licence].
- **No closed dependencies:** external price sources must be replaceable; the rate rule does not depend on a single feed.
- **Support:** after the final report: updating the model on new epochs, and participation in the parameter votes.
- **Handoff:** a simulator README with the procedure for calibrating on fresh chain data.
- **Protocol-facing:** yes (rewards, pricing, fees) — specifications go to Protocol Maintainers for review; implementation is a separate proposal.

## Reporting

- **Interim:** a review sync with the core team.
- **After each item:** a report in Discussions — artifacts, deviations, spending.
- **After the stress tests:** a walkthrough of the results on a community call.
- **Final report:** spending by line, remainder, and a recommendation on implementation.

## Sources

- Bitfury — Gonka: tokenomics improvement proposals, presented at a community AMA (12 November 2026); no public document yet
- Gonka chain data, epochs 14–435: [`epoch_performance_summary`](https://node3.gonka.ai/chain-api/productscience/inference/inference/epoch_performance_summary), and `bitcoin_reward_params` and the other `inference` module [parameters](https://node3.gonka.ai/chain-api/productscience/inference/inference/params)
- [Akash — AEP-76: Burn Mint Equilibrium on Akash](https://akash.network/roadmap/aep-76/)
- [CoinDesk Research — The Incentive Dynamic Engine: a new era for io.net tokenomics](https://coindesk.com/research/the-incentive-dynamic-engine-a-new-era-for-io-net-tokenomics)
- Messari and Escape Velocity — State of DePIN 2025: HNT price and Helium onchain revenue, December 2024 to December 2025, as reported by [Cointelegraph](https://cointelegraph.com/news/depin-revenues-grow-despite-token-losses) (29 January 2026)
- Kalabić, Ballandies, Paruch, Nax, Nigg — [Burn-and-Mint Tokenomics: Deflation and Strategic Incentives](https://doi.org/10.1109/WF-IoT58464.2023.10539427), 2023 IEEE 9th World Forum on Internet of Things (WF-IoT)
