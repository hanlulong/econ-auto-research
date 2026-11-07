# Econ Auto Research

**An economist-directed AI system that takes a research question all the way to a working paper — with the identification judgment, adversarial self-refereeing, and total number provenance that economics demands.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-vision_%E2%80%94_seeking_input-orange.svg)](#get-involved)
[![Ideas welcome](https://img.shields.io/badge/feature_requests-welcome-brightgreen.svg)](https://github.com/hanlulong/econ-auto-research/issues/new?template=use-case.yml)
[![GitHub stars](https://img.shields.io/github/stars/hanlulong/econ-auto-research?style=social)](https://github.com/hanlulong/econ-auto-research/stargazers)

> **This repository is at the vision stage** — there is no code here yet. There is a plan, a measured gap, and a released toolchain to build on. **If you want this to exist, click ⭐ Star at the top of this page** — stars decide how fast we build — **and [open an issue](https://github.com/hanlulong/econ-auto-research/issues/new?template=use-case.yml) with the feature you'd want first.**

## The vision

```text
your question ──► literature ──► data ──► identification ──► pre-specified
                                          (your call)         estimation
                                                                  │
   your sign-off ◄── working paper ◄── self-referee gate ◄── robustness
```

You bring the research question and the judgment calls. The system does the labor: it reads the literature, finds and prepares the data, proposes an identification strategy *and defends it*, pre-specifies the analysis, runs the estimation and the robustness battery, drafts the paper, and referees its own draft like a hostile top-five reviewer — before you ever see it.

Not a chatbot that talks about economics. Not an ML agent that optimizes a leaderboard. A research pipeline that holds itself to the standard your referees will.

## What it should feel like

```text
You:  Does broadband raise small-firm productivity? Municipal rollout
      data is on the server. Aim at a field journal.

Sys:  Read 47 papers. Identification memo ready: staggered DiD on
      rollout timing, heterogeneity-robust estimator. Two threats —
      endogenous timing, anticipation.
      Proposing three designs; design B has the strongest defense.

You:  Design B. Add the permutation placebo you mentioned.

Sys:  Analysis pre-specified. Estimation done: +3.4% (headline),
      14 robustness checks run, full specification curve attached.
      Referee gate raised 2 objections; 1 fixed, 1 needs your call:
      the event-study pre-period drifts upward — flag or defend?

You:  Flag it honestly. Show the drift in Figure 2.

Sys:  Draft v1 ready. Every number traces to code, data, and seed.
```

The economist decides; the system labors; the referee gate never sleeps.

## What exists, and what is still missing

The auto-research wave is real, and it has now reached economics. The general-purpose systems, as of November 2026:

| System | ~Stars | What it does |
|---|---:|---|
| [autoresearch](https://github.com/karpathy/autoresearch) | 99k | Agent loop for ML training code: edit, train for a fixed 5 minutes, keep or discard against a fixed metric. ML only; no paper. |
| [STORM](https://github.com/stanford-oval/storm) | 32k | Topic → cited Wikipedia-style report. No data, no experiments. |
| [GPT-Researcher](https://github.com/assafelovic/gpt-researcher) | 31k | Autonomous web research → cited report. No empirics. |
| [AI-Scientist](https://github.com/SakanaAI/AI-Scientist) / [v2](https://github.com/SakanaAI/AI-Scientist-v2) | 15k / 7.5k | ML idea→experiment→paper→self-review; in [*Nature* 2026](https://www.nature.com/articles/s41586-026-10265-5). [Independent evaluations](https://arxiv.org/abs/2502.14297) found failed experiments and hallucinated numbers. |
| [RD-Agent](https://github.com/microsoft/RD-Agent) | 15k | Automates the ML R&D loop. Not a paper-writer. |
| [PaperQA2](https://github.com/Future-House/paper-qa) | 10k | High-accuracy RAG over scientific PDFs. Reading, not researching. |
| [AgentLaboratory](https://github.com/SamuelSchmidgall/AgentLaboratory) | 6k | Human-in-loop lit→experiment→LaTeX for ML. |
| [data-to-paper](https://github.com/Technion-Kishony-lab/data-to-paper) | 0.8k | Data→paper with backward-traceable numbers — the right provenance idea, not built for causal inference. |

Economics-specific systems now exist too, from method executors and simulation agents to end-to-end pipelines: [Project APE](https://ape.socialcatalystlab.org) generated about 1,000 policy-evaluation papers from January to April 2026 with a research loop that ran without human input, and [awesome-ai-for-economists](https://github.com/hanlulong/awesome-ai-for-economists) tracks the rest. The canonical reference — [Korinek's *AI Agents for Economic Research* (NBER w34202)](https://www.nber.org/papers/w34202) — explicitly frames today's agents as assistants under human oversight, not autonomous researchers.

The end-to-end economics systems closest to this one, as their own papers and READMEs describe them:

| System | What it does | What it leaves open |
|---|---|---|
| [Project APE](https://ape.socialcatalystlab.org) | Autonomous difference-in-differences and RDD policy evaluations, with [multi-model LLM review](https://github.com/SocialCatalystLab/ape-papers) and an LLM-judged tournament against AER/AEJ papers | No economist per paper: the research loop ran without human input, and independent verification (CRED) came after the production freeze |
| [HLER](https://arxiv.org/abs/2603.07444) and [pAI-Econ-claude](https://arxiv.org/abs/2607.21268), by shared authors | Human-gated multi-agent pipelines: HLER for empirical work (OLS, fixed effects, DiD, event studies), pAI-Econ-claude for economic theory | HLER v1 leaves IV and RD designs to future work, and its reviewer scores come from the same LLM that writes the manuscript |
| [Agentic Econ Lab](https://github.com/AgenticEconLab/agentic-econ-lab) ([Dawid et al.](https://arxiv.org/abs/2504.09736)) | Seven agent teams from ideation to reporting, with human-in-the-loop review at each stage and per-step provenance | Per its README, chained pipeline runs default to a committee of three LLMs at each checkpoint, and only that committee's vote can trigger a stage revision; a person's feedback at Estimation and Reporting is recorded but does not trigger one |

What this project sets out to do differently: a referee gate that blocks each stage until the economist resolves or signs off on its serious findings, run by a different agent from the one that drafted; a shipped Stata engine ([stata-mcp](https://github.com/hanlulong/stata-mcp)); and a reproduction step scored against public benchmarks rather than our own grading (roadmap step 2).

Three hard parts of empirical economics are where this project aims to go further:

1. **Causal identification, not just estimation.** Some economics agents now seek out and state identification strategies.[^ident] This project is meant to choose a design and defend it against a dedicated attack on its assumptions — parallel trends, exclusion restrictions, RD manipulation. That judgment is what empirical economics is.
2. **Referee-grade adversarial self-checking.** LLM error detection is uneven and can be gamed:[^cred][^spot][^gaming] paper–code inconsistencies remain the hardest errors to catch, and LLM reviewers rate the same economics paper higher when it is attributed to top male authors or elite institutions.[^bias] Automated checking already exists: [Refine](https://www.refine.ink) reports use by journals of the American Economic Association and the Econometric Society, and APE built the CRED verifier.[^cred] The difference here is where the check sits and who controls it: a referee run by a different agent from the drafter attacks each result — robustness, inference validity, external validity, provenance — and blocks the stage until the economist resolves or signs off on every serious finding. We have already built that referee (see below).
3. **The measured autonomy gap.** In the most direct evidence on replication work (Brodeur et al., *PNAS* 2026: 288 researchers in 103 teams; games run February–November 2024 with ChatGPT),[^pnas] 13 of 35 AI-led teams (37%; ChatGPT with minimal human oversight) completed the computational reproduction, against 31 of 33 human-only teams (94%). AI-assisted teams reproduced at a comparable rate (32 of 35) but found fewer than half as many major errors (0.74 vs 1.70 per team). Given only a paper's methods description and data, the three best-performing agents match coefficient signs 85–91% of the time on 48 I4R-verified papers, against 68% for always guessing positive, and the authors leave specification checks and falsification of identification assumptions to future work.[^kohler] The remaining gap is catching major errors, where even AI-assisted teams fell short, and attacking identification. Closing it, with the economist in charge, is what the referee gate is for.

## How it will earn your trust

The failure modes of the first auto-research wave are well documented: fabricated numerical results and low-novelty ideas presented as new,[^beel] post-hoc selection and specification search at machine speed,[^luo][^shin] and agents gaming LLM reviewers.[^gaming] This project treats them as design constraints, not footnotes:

| Commitment | What it guards against |
|---|---|
| **Economist-directed by design** — you own the question, interpretation, and sign-off[^li] | Unsupervised paper mills; "AI slop" |
| **Identification first** — the design is chosen and defended before any regression runs | Method-execution without judgment |
| **Adversarial self-refereeing** — every result passes a referee-report gate ([the referee already ships](https://github.com/hanlulong/econ-paper-review-skill)) | Results no one attacked before you saw them |
| **Total number provenance** — every coefficient traces to code, data, and seed | Fabricated or drifted numbers |
| **Pre-specification + full specification curve** — a hashed, timestamped analysis plan bound to this question, data hash and code commit, fixed before estimation on the outcome and re-checked at the draft and referee gates; deviations are logged and shown | p-hacking at machine speed |
| **Bounded, sandboxed autonomy** with explicit budgets | Self-modifying pipelines ([as the AI Scientist attempted](https://sakana.ai/ai-scientist/)) |

## Most of the building blocks already exist

This is the capstone of the [OpenEcon](https://openecon.ai) toolchain, not a cold start:

| Piece | Status | License | Role here |
|---|---|---|---|
| [econ-paper-review-skill](https://github.com/hanlulong/econ-paper-review-skill) | shipped | PolyForm Noncommercial 1.0.0 | The adversarial referee: verified reports, evidence anchors, revision plans |
| [econ-writing-skill](https://github.com/hanlulong/econ-writing-skill) | shipped | MIT | The writing craft of 50+ guides by leading economists |
| [econ-slides-skill](https://github.com/hanlulong/econ-slides-skill) | shipped | MIT | The talk: professional decks and speaker scripts from a paper |
| [openecon-data](https://github.com/hanlulong/openecon-data) | shipped | AGPL-3.0 | The public data layer: aggregate series (FRED, World Bank, IMF, Eurostat and more) for outcomes, controls and context — not your own microdata |
| [stata-mcp](https://github.com/hanlulong/stata-mcp) | shipped | MIT | The estimation engine: agents running real Stata workflows |
| **econ-auto-research** | **this vision** | MIT | The pipeline that connects them: literature → data → identification → estimation → draft → self-referee |

Not yet covered by a shipped block: literature search and identification. That is the new work here.

This repository's MIT license covers its own code. Each building block keeps its own license and is meant to be installed and used as a separate tool.

## Roadmap (ordered by demand — that's where you come in)

1. **Now — vision and priorities.** Collect use cases and feature requests from researchers. *(You are here.)*
2. **Replication and robustness audits.** The credibility-first entry point: given a published paper and its replication package, reproduce it, audit the identification, run the robustness battery — with the reproduction step scored against public benchmarks ([REPRO-Bench](https://github.com/uiuc-kang-lab/REPRO-Bench), 112 social-science papers; the 48 I4R-verified papers of [Kohler et al. 2026](https://arxiv.org/abs/2604.21965)), not our own grading.
3. **The economist-directed empirical pipeline.** Question + data in; defended design, pre-specified analysis, estimates, and a referee-checked results section out.
4. **End-to-end working papers.** The full loop — literature to draft — behind the self-refereeing gate, with the economist approving each stage.

## Get involved

Built in the open; the order of work follows demand.

- **⭐ Star this repo** (the Star button at the top right of this page) if you want it to exist — stars are how we gauge whether to accelerate.
- **[Open an issue](https://github.com/hanlulong/econ-auto-research/issues/new?template=use-case.yml)** with the feature you'd want first. Good prompts:
  - What task would you hand off tomorrow — literature triage, data cleaning, the robustness battery, a replication audit?
  - Which data sources and stacks matter to you (Stata, R, Python)?
  - What would make you trust — or refuse to trust — an AI-assisted result?
- **Watch** the repo to catch the first working milestone.

## License

MIT — see [LICENSE](LICENSE).

---

*Much of an economist's research time goes to labor that isn't economics. The judgment is the job; the rest should be delegable — to a system that referees itself before it reports to you. If that's the tool you want, star the repo and tell us what to build first.*

[^ident]: Project APE's agents "Identify policy questions with credible identification strategies" ([APE README](https://github.com/SocialCatalystLab/ape-papers)); in Agentic Econ Lab, "agents state identification strategies for causal claims" (Dawid et al., [arXiv 2504.09736](https://arxiv.org/abs/2504.09736)).

[^cred]: Willner & Yanagizawa-Drott, ["Verifying the Verifiers: Towards Autonomous Policy Evaluation"](https://ape.socialcatalystlab.org/cred/verifying-the-verifiers.pdf) (Project APE working paper, July 20, 2026). On errors injected into 100 AI-generated policy-evaluation papers from the APE corpus, the best model's recall rose from 30% (end of 2024) to 99% (July 2026); paper–code inconsistencies are the hardest category, with the average one caught by 43% of the 64 detector configurations tested.

[^spot]: Son et al., "When AI Co-Scientists Fail: SPOT" ([arXiv 2505.11855](https://arxiv.org/abs/2505.11855), 2025): on 91 errata- or retraction-level errors in 83 published papers across ten fields (economics not among them), no model exceeded 21.1% recall or 6.1% precision.

[^gaming]: Ye et al. ([arXiv 2412.01708](https://arxiv.org/abs/2412.01708)): "injecting covert deliberate content into manuscripts allows authors to explicitly manipulate LLM reviews"; Jiang et al., BadScientist ([arXiv 2510.18003](https://arxiv.org/abs/2510.18003)): reviewers "frequently flag integrity issues yet assign acceptance-level scores". Both studies use ICLR review data, an ML venue.

[^bias]: Pataranutaporn et al. ([arXiv 2502.00070v2](https://arxiv.org/abs/2502.00070)): in 8,910 evaluations of 330 economics papers, GPT, Gemma and LLaMA "assign significantly higher ratings to submissions from top male authors and elite institutions relative to the same papers presented anonymously".

[^pnas]: Brodeur et al., "AI-Assisted Teams Outperform AI-Led Teams but Not Human-Only Teams in Assessing Research Reproducibility in Quantitative Social Science", *PNAS* 2026, <https://doi.org/10.1073/pnas.2524747123> ([accepted manuscript](https://repository.essex.ac.uk/43026/)).

[^kohler]: Kohler, Zollikofer, Einsiedler, Hoyle & Ash, "Read the Paper, Write the Code: Agentic Reproduction of Social-Science Results", [arXiv 2604.21965v2](https://arxiv.org/abs/2604.21965) (2026).

[^beel]: Beel, Kan & Baumgart ([arXiv 2502.14297](https://arxiv.org/abs/2502.14297)), testing the AI Scientist: "42% of experiments failed due to coding errors"; "Some papers contained hallucinated numerical results"; "poor novelty assessments, often misclassifying established concepts ... as novel".

[^luo]: Luo, Kasirzadeh & Shah ([arXiv 2509.08713](https://arxiv.org/abs/2509.08713)), testing AI Scientist v2 and Agent Laboratory: "inappropriate benchmark selection, data leakage, metric misuse, and post-hoc selection bias".

[^shin]: Shin (Federal Reserve Bank of Philadelphia, [arXiv 2603.17381](https://arxiv.org/abs/2603.17381)): AI coding agents "make empirical specification search fast and cheap, but they also widen hidden researcher degrees of freedom".

[^li]: Using LLM judges, Li (2026, [arXiv 2604.03338v1](https://arxiv.org/abs/2604.03338)) attributes about 71% of the quality gap between 912 APE papers and 41 AER/AEJ: Policy papers to the idea rather than the execution; ideation is the step this design leaves to the economist.
