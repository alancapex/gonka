# Proposal: Decision Models on Gonka

Nov 16, 2026

**Proposer:** Ancapex Team

**Status:** draft v0.5 · pre-proposal for discussion (no on-chain vote requested)

## Summary

Gonka needs to grow, and hardware is one of the things holding it back: the GPUs that can serve today's LLMs are scarce, and competition for them is intense. Simpler hardware is available, but the LLMs that fit on it are not what the market wants to buy.

Paradoxically, a new class of models has appeared that runs on modest hardware and is in demand: decision models. We propose to bring this class to Gonka.

This document explains what the class is and the six pieces of work we plan: five to get it running on testnet, and a reference training scheme for later.

## What a decision model is

[TypeSafe AI](https://typesafe.ai/blog/introducing-system-one-models-and-jev), which started the category with its Jev model in September 2026, describes these as models built to make "fast, structured decisions that software can use directly." An LLM writes text that a program then has to interpret. A decision model returns one of the answers the program asked it to choose from, together with how confident it is, in a fraction of a second.

TypeSafe lists the uses: classifying and routing requests, scoring and checking the output of other AI systems, guardrails, processing large volumes of data, and real-time applications where speed matters.

Jev's weights are closed, but open-weight models followed within weeks, from Perplexity, Cloudflare, Baseten and others. They are small: depending on the quality required, they need GPUs with 16 to 80 GB of memory. OpenRouter now lists 27 decision models including entries from OpenAI, Google and Microsoft, and Artificial Analysis benchmarks the category separately. Together they already carry about 6.7T tokens a week, comparable to the entire demand for GLM-5.3 Flash (12.6T), one of the models Gonka serves today.

## What we will do and deliverables

There will be an interim sync with the core team to review the work.

**1. Integrate into confirmation PoC (cPoC).** A host's weight is confirmed during the epoch by confirmation PoC. Hosts that serve decision models on smaller GPUs have to take part in it on the same terms as everyone else, and the check has to coexist with serving decision requests.

*Deliverable:* decision-model hosts pass cPoC on testnet, with their weight confirmed the same way as for LLM hosts.

**2. Integrate into current validation.** Gonka's validation is built around generated text. A decision answer is a set of probabilities, and it has to be checked across the different GPU generations in the network.

*Deliverable:* a validation method for the class, with measured agreement between honest hosts on different hardware.

**3. Integrate into current tokenomics.** The class has to fit the existing economy: how its hosts earn consensus weight next to hosts serving LLMs, and how requests are billed and priced when there are no output tokens.

*Deliverable:* a description of how weight, billing and pricing apply to the class.

**4. Choose the base models.** We will pick a pair of open-weight models of different sizes: one for small cards, one larger. Criteria: open weights under a permissive license, fit for the target hardware, compatibility with the interface the market already uses.

*Deliverable:* two named models, with memory and speed measured on hardware present in the network.

**5. Pull request and testnet.** Implementation of the request path, validation and accounting for the class, tested on testnet with hosts on different hardware.

*Deliverable:* a working class on testnet and a recommendation on whether to take it to a mainnet vote.

**6. Reference training scheme.** Open decision models are built in several ways today: trained heads and adapters on a frozen base model, full fine-tunes, distillation from a larger model. We will describe one reference way to train a decision model from an open base, including how its accuracy and the honesty of its probabilities are evaluated.

*Deliverable:* a documented, reproducible training scheme that can later be integrated with the training mechanism being prepared for Gonka, so the network can produce its own decision models and not only serve third-party ones.

## Payment Request

We request **40,000 USDT and 150,000 GNK** to fund work on all six items.

## Why This Team

Ancapex is a team with experience in, and a focus on, AI and crypto. We are an early Gonka miner, a contributor to Gonka's payment system, and an active member of the community.

**Conflict of interest.** The authors are active hosts and operate a GNK brokerage. We benefit from a new class of paid work in the same way as other hosts and brokers. The models, the weight of the class and all parameter values are decided by separate votes, not by the authors.

## Funded-Work Rules

- **Open source:** specifications in `proposals/` of the main repo; code as a PR to `gonka-ai/gonka`; measurements and the training scheme in [repo, licence].
- **Open models only:** base models with open weights under a permissive licence; no dependency on a vendor's API.
- **Support:** after the testnet launch: fixes, and participation in the votes on models and weight.
- **Handoff:** documentation for hosts on serving the class, and the procedure for adding or replacing a decision model.
- **Protocol-facing:** yes (PoC, validation, rewards, pricing) — review by Protocol Maintainers, testermint, the usual upgrade process.

## Reporting

- Interim: a review sync with the core team.
- After each item: a report in Discussions — artifacts, deviations, spending.
- After the testnet launch: a walkthrough of the results on a community call.
- Final report: spending by line, remainder, and a recommendation on a mainnet vote.

## Sources

- [Artificial Analysis — Decision Index](https://artificialanalysis.ai/evaluations/artificial-analysis-decision-index) and [Decision Index v2](https://artificialanalysis.ai/articles/decision-index-v2) (8 November 2026)
- [OpenRouter](https://openrouter.ai/) model listing, snapshot 2026-11-15
- [TypeSafe AI — Introducing System One models and Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev)
