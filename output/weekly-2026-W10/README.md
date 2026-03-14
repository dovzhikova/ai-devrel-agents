# Weekly Run Output — 2026-W10 (March 10-14)

This directory contains the full output from a live run of the multi-agent system against the real PostHog GitHub repo and social media.

## Run Summary

| Metric | Value |
|--------|-------|
| Issues Triaged (Sage) | 39 |
| Social Mentions (Echo) | 60 |
| Themes Extracted (Iris) | 10 |
| Experiments Designed (Nova) | 3 |
| Content Produced (Kai) | 1 tutorial |
| Status | Complete |

## Files

- `context_2026-W10.json` — Full JSON output from all 6 agents (raw data)

## How It Was Generated

```bash
python -m agents.atlas --weekly-cycle
```

All data is from live API calls:
- **GitHub**: PostHog/posthog repo, issues from past 7 days
- **Reddit/HN/Twitter**: Firecrawl web search for PostHog mentions
- **Theme extraction**: Claude Sonnet via Anthropic API
- **Content generation**: Claude Sonnet via Anthropic API
- **Power analysis**: scipy.stats sample size calculations
