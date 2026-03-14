# Experiment Pre-Registration: Interactive SDK Setup Wizard

**Agent:** Nova (Growth Strategist) | **Status:** Pre-Registration | **Start Date:** March 17, 2026

---

## Hypothesis

Developers abandon onboarding when faced with multi-step manual SDK setup (create API key → download SDK → write boilerplate → test integration). An **interactive, guided setup wizard** that auto-generates language-specific boilerplate code will reduce time-to-first-event from 25 minutes to <10 minutes, increasing activation rate by 15–20%.

---

## Experiment Overview

**Objective:** Reduce onboarding friction for new developers via interactive guided setup.

**Primary Metric:** Time to First Event (minutes)
- Target: Reduce from 25 min (control) to 10 min (variant)

**Secondary Metrics:**
- Activation rate (% of users who send ≥1 event within 24h)
- Funnel completion rate (each onboarding step)
- Free-to-trial conversion rate

**Guardrail Metrics:**
- Customer support tickets related to SDK setup
- Event ingest error rate
- Failed authentication attempts

---

## Sample Size Calculation

**Assumptions:**
- Baseline activation rate: 64%
- Target: 74% (+10 percentage points)
- Significance level (α): 0.05
- Power: 0.80 (β = 0.20)

**Calculation (two-sided test):**
- z_α/2 = 1.96, z_β = 0.84
- p₁ = 0.64, p₂ = 0.74, p̄ = 0.69
- n = (1.96 + 0.84)² × 0.69 × 0.31 / (0.10)²
- n ≈ **414 users per group**
- Total sample required: **828 users**

**Timeline to significance:**
- Expected new users/week: ~200
- Estimated runtime: 4–5 weeks

---

## Power Analysis

| Effect Size | Alpha | Power | Required N/Group | Total |
|---|---|---|---|---|
| 10pp delta (64% → 74%) | 0.05 | 0.80 | 414 | 828 |
| 8pp delta (64% → 72%) | 0.05 | 0.80 | 646 | 1,292 |
| 12pp delta (64% → 76%) | 0.05 | 0.80 | 285 | 570 |

**Decision:** Proceeding with 10pp delta; if underpowered after 3 weeks, we'll extend to 6 weeks.

---

## Control vs Variant Experience

### Control (Status Quo)
1. Create account → directed to docs page
2. Create API key manually (Settings → Keys)
3. Choose language/SDK from dropdown
4. Copy install command
5. Paste boilerplate code from docs
6. Run test event manually
7. Navigate to dashboard to verify
8. Average time: 25 minutes

### Variant (Interactive Wizard)
1. Create account → enter wizard immediately
2. **Interactive step 1:** Wizard generates API key automatically (no manual step)
3. **Interactive step 2:** Language selector with real-time code preview
4. **Interactive step 3:** Copy-paste ready boilerplate (auto-filled with API key)
5. **Interactive step 4:** Embedded terminal/live test environment
6. **Interactive step 5:** Auto-detected event in dashboard with celebration
7. Average time: <10 minutes

---

## Guardrail Metrics

To ensure the variant doesn't degrade the experience:

| Guardrail | Control Baseline | Alert Threshold |
|---|---|---|
| Support tickets (SDK setup) | 8/week | >15/week |
| Event ingest errors | 2% | >5% |
| Failed auth attempts | 3% | >7% |
| User rage quits (session abandon rate) | 12% | >20% |

If any guardrail is triggered, we'll investigate and potentially pause the variant.

---

## Success Criteria

The experiment is deemed successful if:

1. **Primary metric:** Time to first event reduced by ≥50% (25 min → 12.5 min or less)
2. **Activation rate:** Increases from 64% to ≥71% (7pp gain)
3. **No guardrail breaches** across support volume, error rates, or abandonment
4. **Variant sustains** 90%+ of the improvement after variant becomes default

If all three are met, we'll roll out the wizard to 100% of new signups.

---

## Timeline

- **Week 1 (Mar 17–23):** Design + dev sprint, 50% of new users → variant
- **Week 2 (Mar 24–30):** Ramp to 75%, monitor guardrails
- **Week 3 (Mar 31–Apr 6):** Full 50/50 split, assess significance
- **Week 4 (Apr 7–13):** Extend if needed OR prepare rollout to 100%
- **Week 5 (Apr 14–20):** 100% rollout (if successful)

---

## Key Metrics Dashboard

Tracking via PostHog with these custom events:

```
onboarding_started
  → variant: control | wizard

wizard_step_completed
  → step: api_key_generated | language_selected | code_copied | test_executed

time_to_first_event
  → duration_seconds: number

activation_24h
  → activated: true | false
```

---

## Potential Risks & Mitigation

| Risk | Impact | Mitigation |
|---|---|---|
| Variant code generation buggy | High | QA testing with 10 languages before rollout |
| Increased support tickets | Medium | On-call support during ramp phase |
| Users distrust auto-generated code | Low | Include "generated for you" messaging + docs link |
| API key creation fails | High | Fallback to manual key creation in wizard |

---

## Owner & Stakeholders

- **Experiment Owner:** Nova (Growth)
- **Engineering Lead:** [TBD]
- **Product Manager:** [TBD]
- **Success Owner:** [Support lead for guardrails]

---

## Sign-Off

By starting this experiment, we commit to:
- Running the full duration without early stopping (unless guardrails breach)
- Documenting learnings regardless of outcome
- Rolling out or sunletting the variant by end of Q2 2026

