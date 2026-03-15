# AI DevRel Agent System

**A multi-agent AI system that automates developer advocacy — community triage, social listening, feedback synthesis, growth experiments, and content creation — coordinated by a single orchestrator.**

This repo showcases the system's architecture, capabilities, and real output from a live run against the [PostHog](https://github.com/PostHog/posthog) open-source project.

---

## The Problem

Developer Relations teams juggle too many responsibilities across fragmented channels — GitHub issues, Reddit threads, Hacker News discussions, Twitter mentions, documentation gaps, and growth metrics. Critical signals get lost. Content lags behind community pain points. Engagement opportunities expire before anyone notices them.

## The Solution

Six specialized AI agents, each owning one domain, coordinated by an orchestrator that runs a weekly pipeline. Upstream outputs flow downstream — GitHub triage informs feedback synthesis, which informs experiment design, which informs content creation.

```
         ┌──────────────────────────────┐
         │     Atlas (Orchestrator)     │
         │  Coordinates weekly cycle,   │
         │  manages cross-agent context │
         └──┬────┬────┬────┬────┬──────┘
            │    │    │    │    │
    ┌───────┘    │    │    │    └───────┐
    ▼            ▼    │    ▼            ▼
 ┌──────┐  ┌──────┐  │  ┌──────┐  ┌──────┐
 │ Sage │  │ Echo │  │  │ Nova │  │ Kai  │
 │GitHub│  │Social│  │  │Growth│  │Content│
 │Triage│  │Listen│  │  │Exper.│  │Create│
 └──────┘  └──────┘  │  └──────┘  └──────┘
                      ▼
                 ┌──────┐
                 │ Iris │
                 │Feedbk│
                 │Synth.│
                 └──────┘
```

---

## The Agents

### Sage — Community Manager
Triages GitHub issues with priority scoring, sentiment analysis, and churn detection. Categorizes by product area, flags at-risk contributors, and drafts suggested first responses.

### Echo — Social Media Listener
Scans Reddit, Hacker News, and Twitter/X for brand mentions. Classifies sentiment, identifies engagement opportunities (comparison threads, how-to questions), and flags reputation risks.

### Iris — Feedback Synthesizer
Extracts recurring themes from all upstream signals using LLM-based analysis. Ranks pain points by composite score (frequency x severity) and maps them to developer journey stages (discovery → evaluation → onboarding → integration → scaling).

### Nova — Growth Strategist
Designs A/B experiments with statistical rigor — power analysis, sample size calculations, guardrail metrics, and success criteria. Identifies funnel drop-off points and recommends interventions.

### Kai — Content Creator
Writes technical tutorials grounded in the knowledge base and informed by upstream pain points. Produces code examples, CI/CD integration guides, and best practices documentation.

### Atlas — Orchestrator
Coordinates the weekly cycle, brokers cross-agent context, handles retry with exponential backoff + jitter, compiles OKR progress, and archives weekly state.

---

## Weekly Pipeline

| Day | Agent | Input | Output |
|-----|-------|-------|--------|
| **Mon** | Sage | GitHub API (past 7 days) | Triaged issues with priority, sentiment, churn flags |
| **Mon** | Echo | Reddit, HN, Twitter/X | Social mentions, engagement opportunities, reputation risks |
| **Tue** | Iris | Sage triage + Echo social data | Ranked themes, developer journey friction map |
| **Wed** | Nova | Iris themes | A/B experiments with power analysis, funnel insights |
| **Thu** | Kai | All upstream context + knowledge base | Technical tutorial addressing #1 pain point |
| **Fri** | Atlas | All agent outputs | OKR compilation, weekly context archive |

---

## Live Run Results (Week of March 10, 2026)

The system ran against the real PostHog GitHub repo and live social media data. Full JSON output in [`output/weekly-2026-W10/`](output/weekly-2026-W10/).

### Sage: 39 GitHub Issues Triaged

- **4 critical** — security: pickle deserialization, shell=True subprocess
- **3 high priority** — DevEx infrastructure, cloud dev environments
- 31 bugs, 4 questions, 2 feature requests, 1 performance issue
- 1 frustrated contributor detected, 0 churn risks
- Top product areas: analytics (27), data warehouse (5), feature flags (3), session replay (3)

### Echo: 60 Social Mentions Across 3 Platforms

| Platform | Mentions | Positive | Neutral | Negative |
|----------|----------|----------|---------|----------|
| Reddit | 20 | 7 | 12 | 1 |
| Hacker News | 20 | 3 | 17 | 0 |
| Twitter/X | 20 | 4 | 16 | 0 |
| **Total** | **60** | **14** | **45** | **1** |

**9 engagement opportunities** surfaced:
- *"PostHog vs BetterStack"* (r/devops) — provide honest comparison
- *"How to integrate PostHog to a website?"* (r/learnprogramming) — share tutorial
- *"PostHog vs Google Analytics for Lovable apps?"* (r/lovable) — comparison response
- *"Is PostHog a good alternative to Google Analytics?"* (r/DigitalMarketing) — comparison
- *"Heap vs Full Story vs PostHog"* (r/ProductManagement) — comparison
- *"PostHog as a data warehouse"* (r/dataanalysis) — share capabilities

**0 reputation risks** flagged.

### Iris: 10 Themes Extracted via LLM

| # | Theme | Frequency | Severity | Composite Score |
|---|-------|-----------|----------|-----------------|
| 1 | Developer Experience Infrastructure Overhaul | 23 | 7.5 | 172.5 |
| 2 | Critical Security Vulnerabilities in Core Code | 3 | 9.5 | 28.5 |
| 3 | Feature Flags UX and Functionality Gaps | 3 | 5.0 | 15.0 |
| 4 | UI Polish and Beta Tag Cleanup | 4 | 3.5 | 14.0 |
| 5 | Workflow Testing Reliability Issues | 1 | 6.5 | 6.5 |
| 6 | Privacy and Tracking Detection Concerns | 1 | 6.0 | 6.0 |
| 7 | Advanced Filtering Gaps in Analytics | 1 | 5.5 | 5.5 |
| 8 | Frontend Framework Modernization (React 19) | 1 | 5.0 | 5.0 |
| 9 | Temporal Infrastructure Maintenance | 1 | 4.0 | 4.0 |
| 10 | Documentation Accuracy Issues | 1 | 2.0 | 2.0 |

**Developer journey friction map:**

| Stage | Friction Score | Drop-off Risk | Pain Points |
|-------|---------------|---------------|-------------|
| Discovery | 0.0 | Low | — |
| Evaluation | 2.0 | Low | Documentation accuracy |
| Onboarding | 6.1 | **Medium** | DevEx overhaul, security vulns, UI polish, workflow testing, privacy, filtering, Temporal |
| Integration | 5.0 | **Medium** | Feature flags UX gaps |
| Scaling | 5.0 | **Medium** | React 19 modernization |

### Nova: 3 Experiments Designed

| Experiment | Primary Metric | Sample Size/Arm | Duration | MDE |
|------------|---------------|-----------------|----------|-----|
| DevEx Infrastructure Activation | `development_infrastructure_activation_rate` | 2,402 | 10 days | 3.0pp |
| Security Remediation Impact | `security_activation_rate` | 2,402 | 10 days | 3.0pp |
| Feature Flags UX Improvement | `feature_flags_activation_rate` | 906 | 4 days | 5.0pp |

**Funnel insight**: 70.1% drop-off at `feature_flag_created` stage — recommended qualitative research + friction reduction experiment.

### Kai: Full Tutorial Generated

**"How to Build Reliable Feature Flag Testing into Your CI/CD Pipeline"**

A complete technical tutorial (~350 lines) covering:
- Test matrix pattern for feature flag combinations
- Local flag overrides for PostHog SDK (JavaScript + Python)
- GitHub Actions CI integration
- Multivariate flag testing
- Flag coverage reporting

The tutorial directly addresses the #3 theme (Feature Flags UX Gaps) identified by Iris.

### OKR Summary

| Metric | Value |
|--------|-------|
| Issues triaged | 39 |
| Social mentions found | 60 |
| Themes identified | 10 |
| Experiments designed | 3 |
| Content produced | 1 |
| Status | **Complete** |

---

## Architecture

### Cross-Agent Context Flow

Each agent's output feeds into a shared context object that downstream agents can access:

```
Sage (GitHub triage)  ──┐
                        ├──► Iris (theme extraction) ──► Nova (experiments)
Echo (social listening) ─┘                            ──► Kai (content)
```

### Key Design Decisions

- **Hub-and-spoke, not peer-to-peer** — Atlas as single orchestrator makes the data flow predictable and debugging straightforward
- **LLM-based extraction, not regex** — Theme extraction and sentiment analysis use Claude for accuracy on varied text
- **Graceful degradation** — Sage and Echo work without LLM credits; Iris/Kai skip LLM features when unavailable
- **Retry with backoff + jitter** — Failed agent delegations retry up to 2x with exponential backoff
- **Retargetable** — Swap the knowledge base and API client to point at any DevTools product

### Tech Stack

| Component | Choice |
|-----------|--------|
| Language | Python 3.12+ (async/await, dataclasses) |
| LLM | Claude Sonnet via Anthropic API |
| HTTP | httpx (async) |
| Search | Firecrawl (primary) + Brave Search (fallback) |
| Statistics | scipy (power analysis, Bayesian evaluation) |
| Testing | pytest + pytest-asyncio + respx (198 tests, 87% coverage) |
| Tool Protocol | MCP (Model Context Protocol) |

---

## Sample Deliverables

These are outputs produced by the agent system:

| Agent | Output | File |
|-------|--------|------|
| Sage | Weekly Community Triage Report | [`deliverables/community-triage.md`](deliverables/community-triage.md) |
| Iris | Developer Feedback Synthesis | [`deliverables/feedback-synthesis.md`](deliverables/feedback-synthesis.md) |
| Nova | Experiment Pre-Registration | [`deliverables/growth-experiment.md`](deliverables/growth-experiment.md) |
| Kai | Technical Tutorial | [`deliverables/tutorial.md`](deliverables/tutorial.md) |
| Atlas | System Architecture Overview | [`deliverables/portfolio-summary.md`](deliverables/portfolio-summary.md) |

---

Built by [Daria Dovzhikova](mailto:dovzhikova@gmail.com)
