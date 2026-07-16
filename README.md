# Econ Auto Research

**An economist-directed AI system that takes a research question all the way to a working paper — with the identification judgment, adversarial self-refereeing, and number provenance that economics demands.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status: vision](https://img.shields.io/badge/status-vision%20%E2%80%94%20seeking%20input-orange.svg)](#get-involved)

> **This repository is at the vision stage.** There is no code here yet — there is a plan, a genuine gap, and a working toolchain to build on. If you want this to exist, [star the repo](#get-involved) and [tell us what to build first](#get-involved).

## The vision

You bring the research question and the judgment calls. The system does the labor: it reads the literature, finds and prepares the data, proposes an identification strategy *and defends it*, runs the estimation and the robustness battery, drafts the paper, referees its own draft like a hostile top-five reviewer — and hands you a working paper in which every number is traceable to code and data.

Not a chatbot that talks about economics. Not an ML agent that optimizes a leaderboard metric. A research pipeline that holds itself to the standard your referees will.

## Why this doesn't exist yet

The auto-research wave is real, and none of it does economics. As of mid-2026:

| System | ~Stars | What it does |
|---|---:|---|
| [STORM](https://github.com/stanford-oval/storm) | 30k | Topic → cited Wikipedia-style report. No data, no experiments. |
| [GPT-Researcher](https://github.com/assafelovic/gpt-researcher) | 28k | Autonomous web research → cited report. No empirics. |
| [AI-Scientist](https://github.com/SakanaAI/AI-Scientist) / [v2](https://github.com/SakanaAI/AI-Scientist-v2) | 14k / 7k | ML idea→experiment→paper→self-review. Independent evaluations found failed experiments and hallucinated numbers. |
| [RD-Agent](https://github.com/microsoft/RD-Agent) | 14k | Automates the ML R&D loop. Not a paper-writer. |
| [PaperQA2](https://github.com/Future-House/paper-qa) | 9k | High-accuracy RAG over scientific PDFs. Reading, not researching. |
| [AgentLaboratory](https://github.com/SamuelSchmidgall/AgentLaboratory) | 6k | Human-in-loop lit→experiment→LaTeX for ML. |
| [data-to-paper](https://github.com/Technion-Kishony-lab/data-to-paper) | 0.8k | Data→paper with backward-traceable numbers — the right provenance idea, not built for causal inference. |

The economics-specific tools are early and narrow: method executors and simulation agents, a few hundred stars each. The canonical reference — [Korinek's *AI Agents for Economic Research* (NBER w34202)](https://www.nber.org/papers/w34202) — explicitly frames today's agents as assistants under human oversight, not autonomous researchers.

Three gaps are unclaimed, and they are exactly the hard parts of empirical economics:

1. **Causal identification, not just estimation.** Existing agents can *run* a named method. None reasons about whether parallel trends hold, whether an instrument's exclusion restriction is credible, whether the RD is manipulated — and none *chooses and defends* a design. That judgment is what empirical economics is.
2. **Referee-grade adversarial self-checking.** LLM reviewers under-detect methodological flaws and are gameable ([a 9,000-paper economics study](https://arxiv.org/abs/2502.00070) documents both). A system that attacks its own results — robustness, inference validity, external validity, provenance — before a human ever sees them is a real niche, and we have already built the referee (see below).
3. **The measured autonomy gap.** In the most direct evidence to date ([Brodeur et al., *PNAS* 2026](https://www.pnas.org/doi/10.1073/pnas.2524747123) — 288 researchers, 103 teams in AI replication games), fully autonomous AI reproduced only ~37% of results, AI-led teams trailed human-only teams by ~57 percentage points, and humans caught more of the *critical* errors. Meanwhile the reproduction benchmarks that AI does well on ([~91% coefficient-sign agreement](https://arxiv.org/abs/2604.21965)) explicitly exclude identification, robustness, and referee judgment from scope. That is the gap this project exists to close — deliberately, with the economist in charge.

## What "economics-grade" means here

The failure modes of the first auto-research wave are well documented: fabricated results, hallucinated citations, low-novelty "slop", specification search at machine speed, and agents gaming their own reviewers. This project treats those as design constraints, not footnotes:

- **Economist-directed by design.** You own the question, the interpretation, and the sign-off. The system owns the labor. Autonomy grows only as verification does.
- **Identification first.** The design conversation (what varies, what's assumed, what would break it) happens before any regression runs — and is documented in the paper.
- **Adversarial self-refereeing.** Every result passes through a referee-report gate before it reaches you — the same technology as [econ-paper-review-skill](https://github.com/hanlulong/econ-paper-review-skill), which already writes verified, evidence-anchored referee reports.
- **Total number provenance.** Every coefficient in the draft traces to code, data, and seed. No traceability, no claim.
- **Pre-specification over specification search.** Analysis plans are declared before estimation; the full specification curve is reported, not the flattering corner of it.
- **Bounded, sandboxed autonomy.** Agents run in controlled environments with explicit budgets — no self-modifying pipelines.

## The building blocks already exist

This is the capstone of the [OpenEcon](https://openecon.ai) toolchain, not a cold start:

| Piece | Status | Role here |
|---|---|---|
| [econ-paper-review-skill](https://github.com/hanlulong/econ-paper-review-skill) | shipping | The adversarial referee: verified reports, evidence anchors, revision plans |
| [econ-writing-skill](https://github.com/hanlulong/econ-writing-skill) | shipping | The writing craft of 50+ guides by leading economists |
| [econ-slides-skill](https://github.com/hanlulong/econ-slides-skill) | shipping | The talk: professional decks and speaker scripts from a paper |
| [stata-mcp](https://github.com/hanlulong/stata-mcp) | shipping | The estimation engine: agents running real Stata workflows |
| **econ-auto-research** | **this vision** | The pipeline that connects them: literature → data → identification → estimation → draft → self-referee |

## Roadmap (subject to your input)

1. **Now — vision and priorities.** Collect use cases and feature requests from researchers (that's this README).
2. **Replication and robustness audits.** The credibility-first entry point: given a published paper and its replication package, reproduce it, audit the identification, and run the robustness battery — measured against the public replication benchmarks.
3. **The economist-directed empirical pipeline.** Question + data in; design proposal, pre-specified analysis, estimates, and a referee-checked results section out.
4. **End-to-end working papers.** The full loop — literature to draft — behind the self-refereeing gate, with the economist approving each stage.

## Get involved

This project will be built in the open, and the order of work will follow demand:

- **⭐ Star this repo** if you want it to exist — stars are how we gauge whether to accelerate.
- **[Open an issue](../../issues/new)** describing the feature you'd want first: What task would you hand off tomorrow — literature triage? data cleaning? the robustness battery? a replication audit? Which data sources and stacks matter to you (Stata, R, Python)? What would make you trust — or refuse to trust — an AI-assisted result?
- Watch the repo for the first working milestone.

## License

MIT — see [LICENSE](LICENSE).
