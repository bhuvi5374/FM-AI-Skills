# Journey: Gradual Rollout Planner

## Overview
**Goal:** Create a safe, incremental rollout plan for feature flags with checkpoints, monitoring, and rollback criteria to minimize production risk.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Help me safely roll out this feature to production"
- "Create a gradual rollout plan for my flag"
- "What's the safest way to deploy this feature?"
- "Plan a phased rollout with monitoring"

**Success Criteria:**
- Rollout plan created with incremental percentages
- Monitoring checkpoints defined
- Rollback criteria established
- Schedule set for rollout phases
- Team notified of plan
- User understands what to monitor at each phase

## User Persona
- **Role:** Developer, Product Manager, DevOps Engineer
- **Pain Point:**
  - Fear of breaking production with new features
  - Unsure what rollout percentages to use
  - Don't know how long to wait between phases
  - Unclear what metrics to monitor
  - Need rollback plan but don't have one
  - Want to balance speed with safety

- **Current Manual Process:**
  1. Deploy to 100% immediately (risky)
  2. Or pick random percentages without plan
  3. No clear monitoring or rollback criteria
  4. Wing it when issues arise

  Time: Hours of stress, potential production incidents

## Agentic Happy Path

### Step 1: Assess Feature Risk
**What the agent does:**
- Ask about feature criticality
- Identify blast radius (what breaks if it fails)
- Check feature complexity
- Determine rollback difficulty
- Recommend risk level (low/medium/high)

**Context needed:**
- Feature description
- Systems affected
- User-facing impact
- Historical incident data

**Example:**
```
Agent: "Let's assess the risk of this feature.
What does feature_new_checkout_page affect?"

User: "Our entire checkout process"

Agent: "That's a critical path! If it fails:
- Users can't complete purchases
- Revenue directly impacted
- High visibility

Risk Level: HIGH

Recommendation: Conservative gradual rollout
with extensive monitoring at each phase.
```

### Step 2: Design Rollout Phases
**What the agent does:**
- Suggest appropriate rollout percentages
- Determine phase duration
- Identify target groups for early phases
- Set decision points between phases
- Explain rationale for each phase

**Context needed:**
- Risk assessment
- Total user base size
- Team availability for monitoring
- Business constraints (weekends, holidays)

**Example:**
```
Recommended Rollout Plan:

Phase 1: Internal Testing (0.1% - Internal IPs)
Duration: 24 hours
Target: ~50 internal users
Goal: Catch obvious bugs before real users

Phase 2: Early Adopters (1%)
Duration: 48 hours
Target: ~1,000 users (beta_users group)
Goal: Real-world validation with forgiving users

Phase 3: Small Rollout (10%)
Duration: 72 hours
Target: ~10,000 users (random)
Goal: Detect performance issues at scale

Phase 4: Medium Rollout (50%)
Duration: 5 days
Target: ~50,000 users
Goal: Validate with majority traffic

Phase 5: Full Rollout (100%)
Duration: Ongoing
Target: All users
Goal: Complete migration

Total timeline: ~2 weeks
```

### Step 3: Define Monitoring Checkpoints
**What the agent does:**
- Identify key metrics to monitor
- Set acceptable thresholds
- Define automatic rollback triggers
- Create monitoring checklist per phase
- Set up alerting

**Context needed:**
- Available monitoring tools
- Baseline metrics
- SLA requirements
- Critical user flows

**Example:**
```
Monitoring Checklist:

Phase 1 (0.1% - Internal):
□ Zero errors in application logs
□ Page load time < 2s (baseline: 1.5s)
□ All internal users can complete checkout
□ No console errors reported

Phase 2 (1% - Beta Users):
□ Error rate < 0.1% (baseline: 0.05%)
□ Conversion rate >= baseline (95% confidence)
□ Support tickets < 5 per day about checkout
□ User feedback sentiment positive

Phase 3-5 (10%, 50%, 100%):
□ P95 latency < baseline + 100ms
□ Error rate < baseline + 0.1%
□ Conversion rate >= baseline
□ Revenue per user >= baseline

AUTO-ROLLBACK Triggers:
🚨 Error rate > 1%
🚨 Conversion rate drops > 10%
🚨 P95 latency > baseline + 500ms
🚨 Revenue drop > 5%
```

### Step 4: Establish Rollback Criteria
**What the agent does:**
- Define what triggers rollback
- Create rollback procedure
- Identify rollback timeframe
- Document communication plan
- Set up automatic rollback if possible

**Context needed:**
- Incident response process
- On-call rotation
- Communication channels
- Rollback complexity

**Example:**
```
Rollback Plan:

WHEN TO ROLLBACK:
1. Auto-triggers hit (error rate, latency, conversion)
2. Critical bug reported by 3+ users
3. Revenue impact detected
4. On-call engineer judgment call

HOW TO ROLLBACK:
1. Immediate: Set flag to 0% in production
   (Takes effect in < 30 seconds via SDK)
2. Monitor recovery (5-10 minutes)
3. Notify team in #incidents channel
4. Post-mortem within 24 hours

ROLLBACK DECISION MAKER:
- Phase 1-2: Any engineer
- Phase 3-4: Lead engineer approval
- Phase 5: Product + Engineering approval

Rollback is ALWAYS SAFE - feature flag
protects us. Don't hesitate if issues arise.
```

### Step 5: Schedule & Execute Rollout
**What the agent does:**
- Create rollout schedule
- Set calendar reminders
- Generate runbook
- Notify stakeholders
- Provide execution checklist

**Context needed:**
- Team calendar
- Business hours
- Blackout dates
- Approval workflows

**Example:**
```
Rollout Schedule:

Monday, Mar 18, 10:00 AM → Phase 1 (0.1%)
✓ On-call: Alice
✓ Monitor: Next 24 hours
✓ Decision: Tue Mar 19, 10 AM

Wednesday, Mar 20, 10:00 AM → Phase 2 (1%)
✓ On-call: Bob
✓ Monitor: 48 hours
✓ Decision: Fri Mar 22, 10 AM

Monday, Mar 25, 10:00 AM → Phase 3 (10%)
✓ On-call: Carol
✓ Monitor: 72 hours
✓ Decision: Thu Mar 28, 10 AM

(Full schedule created with calendar invites)

Execution Runbook: [Link to detailed steps]
Slack notifications: #feature-releases
Stakeholder email sent ✓
```

## Required Skills

### Core Skills
- [ ] **risk-assessor** - Evaluates feature risk level
  - Analyze feature criticality
  - Determine blast radius
  - Calculate rollback difficulty
  - Recommend risk level

- [ ] **rollout-phase-designer** - Creates rollout phases
  - Suggest percentage increments
  - Calculate phase durations
  - Identify target groups
  - Explain phase rationale

- [ ] **monitoring-checkpoint-creator** - Defines what to monitor
  - Identify key metrics
  - Set thresholds
  - Create monitoring checklists
  - Configure alerting

- [ ] **rollback-planner** - Establishes rollback procedures
  - Define rollback triggers
  - Create rollback procedure
  - Set decision makers
  - Document communication plan

- [ ] **rollout-scheduler** - Creates execution timeline
  - Build rollout schedule
  - Consider business constraints
  - Set reminders
  - Generate runbook

### Supporting Skills
- [ ] **baseline-metrics-analyzer** - Gathers current performance data
- [ ] **target-group-recommender** - Suggests early rollout groups
- [ ] **alert-configurator** - Sets up monitoring alerts
- [ ] **runbook-generator** - Creates detailed execution guide
- [ ] **stakeholder-notifier** - Communicates plan to team

## Constraints & Guardrails

**Must NOT do:**
- Skip risk assessment
- Rollout to 100% immediately for critical features
- Proceed to next phase with unresolved issues
- Roll out during blackout periods (Black Friday, etc.)
- Ignore monitoring during rollout

**Must ALWAYS do:**
- Start with small percentage (<1%)
- Define rollback triggers upfront
- Monitor metrics at each phase
- Wait sufficient time between phases
- Have on-call coverage during rollout
- Document rollback procedure
- Notify stakeholders of plan
- Get approval for high-risk rollouts

## Edge Cases

1. **Case:** Feature needs instant rollout (emergency fix)
   **Handling:** Allow fast-track with extra monitoring, document exception

2. **Case:** No baseline metrics available (new feature)
   **Handling:** Use proxy metrics, conservative rollout, extra manual validation

3. **Case:** Different rollout needed per region
   **Handling:** Create per-region rollout plans, coordinate timing

4. **Case:** Feature requires coordinated backend deployment
   **Handling:** Deploy backend first, wait for stability, then rollout flag

5. **Case:** Rollout must complete by deadline
   **Handling:** Work backwards from deadline, compress phases cautiously

6. **Case:** Issue found mid-rollout at 30%
   **Handling:** Pause rollout, don't increase %, fix issue, resume or rollback

7. **Case:** Metrics inconclusive (small sample size)
   **Handling:** Wait longer at current phase, gather more data, then decide

## Success Metrics
- [ ] Zero production incidents from planned rollouts
- [ ] Rollout completion time as planned (±20%)
- [ ] Issues detected early (Phase 1-2): 90%+
- [ ] Rollback rate: <5% of rollouts
- [ ] User satisfaction during rollout: No degradation
- [ ] Team confidence in process: 4.5+ stars

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Follows: JOURNEY-07-flag-testing-assistant
- Works with: JOURNEY-09-rollout-impact-analyzer, JOURNEY-10-flag-approval-request-assistant
- Precedes: JOURNEY-11-flag-health-audit (for ongoing monitoring)
