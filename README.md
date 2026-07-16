# Econ Auto Research

**An economist-directed AI system that takes a research question all the way to a working paper — with the identification judgment, adversarial self-refereeing, and total number provenance that economics demands.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-vision_%E2%80%94_seeking_input-orange.svg)](#get-involved)
[![Ideas welcome](https://img.shields.io/badge/feature_requests-welcome-brightgreen.svg)](../../issues/new)

> **This repository is at the vision stage** — there is no code here yet. There is a plan, a measured gap, and a shipping toolchain to build on. **If you want this to exist, [star the repo](#get-involved)** — stars decide how fast we build — **and [open an issue](../../issues/new) with the feature you'd want first.**

## The vision

```text
your question ──► literature ──► data ──► identification ──► pre-specified
                                                              estimation
                                                                  │
   your sign-off ◄── working paper ◄── self-referee gate ◄── robustness
```

You bring the research question and the judgment calls. The system does the labor: it reads the literature, finds and prepares the data, proposes an identification strategy *and defends it*, pre-registers the analysis, runs the estimation and the robustness battery, drafts the paper, and referees its own draft like a hostile top-five reviewer — before you ever see it.

Not a chatbot that talks about economics. Not an ML agent that optimizes a leaderboard. A research pipeline that holds itself to the standard your referees will.

## What it should feel like

```text
You:  Does broadband raise small-firm productivity? Municipal rollout
      data is on the server. Aim at a field journal.

Sys:  Read 47 papers. Identification memo ready: staggered DiD on
      rollout timing. Two threats — endogenous timing, anticipation.
      Proposing three designs; design B has the strongest defense.

You:  Design B. Add the permutation placebo you mentioned.

Sys:  Analysis pre-specified. Estimation done: +3.4% (headline),
      robustness battery 14/14, full specification curve attached.
      Referee gate raised 2 objections; 1 fixed, 1 needs your call:
      the event-study pre-period drifts upward — flag or defend?

You:  Flag it honestly. Show the drift in Figure 2.

Sys:  Draft v1 ready. Every number traces to code, data, and seed.
```

The economist decides; the system labors; the referee gate never sleeps.

## Why this doesn't exist yet

The auto-research wave is real — and none of it does economics. As of mid-2026:

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
2. **Referee-grade adversarial self-checking.** LLM reviewers under-detect methodological flaws and are gameable ([a 9,000-paper economics study](https://arxiv.org/abs/2502.00070) documents both). A system that attacks its own results — robustness, inference validity, external validity, provenance — before a human ever sees them is a real niche. We have already built that referee (see below).
3. **The measured autonomy gap.** In the most direct evidence to date ([Brodeur et al., *PNAS* 2026](https://doi.org/10.1073/pnas.2524747123) — 288 researchers, 103 teams in AI replication games), fully autonomous AI reproduced only ~37% of results, AI-led teams trailed human-only teams by ~57 percentage points, and humans caught more of the *critical* errors. The reproduction tasks AI already does well ([~91% coefficient-sign agreement](https://arxiv.org/abs/2604.21965)) explicitly exclude identification, robustness, and referee judgment. That measured gap is what this project exists to close — deliberately, with the economist in charge.

## What "economics-grade" means here

The failure modes of the first auto-research wave are well documented: fabricated results, hallucinated citations, low-novelty "slop", specification search at machine speed, agents gaming their own reviewers. This project treats them as design constraints, not footnotes:

| Commitment | What it rules out |
|---|---|
| **Economist-directed by design** — you own the question, interpretation, and sign-off | Unsupervised paper mills; "AI slop" |
| **Identification first** — the design is chosen and defended before any regression runs | Method-execution without judgment |
| **Adversarial self-refereeing** — every result passes a referee-report gate ([the referee already ships](https://github.com/hanlulong/econ-paper-review-skill)) | Results no one attacked before you saw them |
| **Total number provenance** — every coefficient traces to code, data, and seed | Fabricated or drifted numbers |
| **Pre-specification + full specification curve** | p-hacking at machine speed |
| **Bounded, sandboxed autonomy** with explicit budgets | Self-modifying pipelines |

## The building blocks already exist

This is the capstone of the [OpenEcon](https://openecon.ai) toolchain, not a cold start:

| Piece | Status | Role here |
|---|---|---|
| [econ-paper-review-skill](https://github.com/hanlulong/econ-paper-review-skill) | shipping | The adversarial referee: verified reports, evidence anchors, revision plans |
| [econ-writing-skill](https://github.com/hanlulong/econ-writing-skill) | shipping | The writing craft of 50+ guides by leading economists |
| [econ-slides-skill](https://github.com/hanlulong/econ-slides-skill) | shipping | The talk: professional decks and speaker scripts from a paper |
| [stata-mcp](https://github.com/hanlulong/stata-mcp) | shipping | The estimation engine: agents running real Stata workflows |
| **econ-auto-research** | **this vision** | The pipeline that connects them: literature → data → identification → estimation → draft → self-referee |

## Roadmap (ordered by demand — that's where you come in)

1. **Now — vision and priorities.** Collect use cases and feature requests from researchers. *(You are here.)*
2. **Replication and robustness audits.** The credibility-first entry point: given a published paper and its replication package, reproduce it, audit the identification, run the robustness battery — measured against the public replication benchmarks, not our own grading.
3. **The economist-directed empirical pipeline.** Question + data in; defended design, pre-specified analysis, estimates, and a referee-checked results section out.
4. **End-to-end working papers.** The full loop — literature to draft — behind the self-refereeing gate, with the economist approving each stage.

## Get involved

Built in the open; the order of work follows demand.

- **⭐ Star this repo** if you want it to exist — stars are how we gauge whether to accelerate.
- **[Open an issue](../../issues/new)** with the feature you'd want first. Good prompts:
  - What task would you hand off tomorrow — literature triage, data cleaning, the robustness battery, a replication audit?
  - Which data sources and stacks matter to you (Stata, R, Python)?
  - What would make you trust — or refuse to trust — an AI-assisted result?
- **Watch** the repo to catch the first working milestone.

## License

MIT — see [LICENSE](LICENSE).

---

*Economists spend most of their research hours on labor that isn't economics. The judgment is the job; the rest should be delegable — to a system that referees itself before it reports to you. If that's the tool you want, star the repo and tell us what to build first.*
