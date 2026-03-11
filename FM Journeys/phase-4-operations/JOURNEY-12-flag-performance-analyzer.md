# Journey: Flag Performance Analyzer

## Overview
**Goal:** Deep dive into individual flag performance metrics, trends, and usage patterns to make data-driven decisions about flag management.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "How is this flag performing?"
- "Show me metrics for feature_checkout_page"
- "Analyze performance trends for this flag"
- "Is this flag being used as expected?"

**Success Criteria:**
- Complete performance metrics displayed
- Trends visualized over time
- Usage patterns identified
- Anomalies flagged
- Recommendations provided
- User understands flag effectiveness

## User Persona
- **Role:** Product Manager, Engineering Manager, Data Analyst
- **Pain Point:**
  - Can't tell if flag is being used correctly
  - Need data to decide next steps (increase rollout, rollback, cleanup)
  - Unclear if flag improves or hurts metrics
  - Want to optimize flag configuration
  - Need to justify flag decisions with data

- **Current Manual Process:**
  1. Check FM dashboard for basic stats
  2. Query analytics system separately
  3. Manual correlation of metrics
  4. Create spreadsheets to track trends

  Time: 30-60 minutes per flag, incomplete view

## Agentic Happy Path

### Step 1: Identify Flag & Time Period
**What the agent does:**
- Ask which flag to analyze
- Determine analysis time period
- Check if flag has sufficient data
- Set baseline comparison period

**Context needed:**
- Flag name
- Historical data availability
- Default time periods (7d, 30d, 90d)

**Example:**
```
Agent: "Which flag do you want to analyze?"
User: "feature_new_checkout_page"

Agent: "Time period for analysis?"
User: "Last 30 days"

Agent: "Checking data availability...
✓ Flag created 90 days ago
✓ Sufficient data for 30-day analysis
✓ Baseline: Previous 30 days (60-90 days ago)

Starting performance analysis..."
```

### Step 2: Gather Performance Metrics
**What the agent does:**
- Collect impression counts
- Gather rollout history
- Pull error rates
- Check latency metrics
- Review conversion rates
- Collect user feedback

**Context needed:**
- CloudBees FM API access
- Application monitoring data
- Analytics system integration

**Example:**
```
Performance Metrics (Last 30 days):

Impressions:
- Total: 2.4M impressions
- Average: 80,000/day
- Trend: +15% vs previous period

Rollout History:
Week 1: 10% rollout
Week 2: 25% rollout
Week 3: 50% rollout
Week 4: 75% rollout
Current: 75% (stable)

Environment Breakdown:
- Production: 2.1M impressions (87%)
- Staging: 250K impressions (10%)
- Development: 50K impressions (3%)

Flag ON Rate:
- Evaluation: 2.4M times
- Result ON: 1.8M (75% - matches rollout ✓)
- Result OFF: 600K (25%)
```

### Step 3: Analyze Performance Trends
**What the agent does:**
- Plot metrics over time
- Identify trends (improving, degrading, stable)
- Detect anomalies or spikes
- Compare to baseline
- Flag concerning patterns

**Context needed:**
- Time series data
- Statistical analysis capabilities
- Baseline metrics

**Example:**
```
Trend Analysis:

Latency (P95):
Baseline: 1.5s
Current: 1.2s (-20%) ✓ IMPROVED

Week 1: 1.45s
Week 2: 1.38s
Week 3: 1.25s
Week 4: 1.20s
Trend: Steadily improving ↓

Error Rate:
Baseline: 0.08%
Current: 0.04% (-50%) ✓ IMPROVED

Anomalies detected:
📌 March 12, 2pm: Error spike to 0.3%
   Duration: 15 minutes
   Cause: Brief API timeout (resolved)
   Impact: Minimal (15 min window)

Conversion Rate (Checkout completion):
Baseline: 4.2%
Current: 4.8% (+14%) ✓ IMPROVED

This flag has POSITIVE impact on all key metrics!
```

### Step 4: User Segmentation Analysis
**What the agent does:**
- Break down usage by user segments
- Compare performance across segments
- Identify which users see most value
- Check targeting effectiveness

**Context needed:**
- User segmentation data
- Target group definitions
- Custom property values

**Example:**
```
Segment Analysis:

By User Type:
┌─────────────┬────────────┬──────────┬────────────┐
│ Segment     │ Users      │ Conv Rate│ Latency    │
├─────────────┼────────────┼──────────┼────────────┤
│ Beta Users  │ 5K (10%)   │ 5.2%     │ 1.1s ✓     │
│ Premium     │ 20K (40%)  │ 5.5%     │ 1.0s ✓✓    │
│ Free        │ 25K (50%)  │ 4.1%     │ 1.3s       │
└─────────────┴────────────┴──────────┴────────────┘

Insight: Premium users see highest benefit
Recommendation: Consider targeting premium first
in future rollouts

By Geography:
- US: 4.9% conversion (above avg)
- EU: 4.7% conversion (average)
- APAC: 4.5% conversion (slightly below)

Insight: Performance consistent across regions
```

### Step 5: Generate Recommendations
**What the agent does:**
- Synthesize findings into actionable insights
- Recommend next steps (increase rollout, optimize, cleanup)
- Identify opportunities for improvement
- Provide decision support

**Context needed:**
- Business goals
- Risk tolerance
- Team priorities

**Example:**
```
Performance Summary & Recommendations:

OVERALL HEALTH: EXCELLENT ✓

Key Findings:
✓ Latency improved 20%
✓ Error rate cut in half
✓ Conversion rate up 14%
✓ No significant issues
✓ Stable at 75% for 1 week

Recommendations:

1. INCREASE ROLLOUT TO 100% 🚀
   Confidence: HIGH
   Reason: All metrics improved, stable for 1 week
   Timeline: Within 48 hours
   Risk: LOW

2. Make flag permanent (consider cleanup)
   Timeline: After 14 days at 100%
   Action: Remove old code path, delete flag

3. Apply learnings to future features
   - New checkout flow is superior
   - Premium users adopt fastest
   - Consider premium-first rollouts

Next Steps:
□ Request approval for 100% rollout
□ Schedule cleanup in 2 weeks
□ Document success for team learning
```

## Required Skills

### Core Skills
- [ ] **metrics-collector** - Gathers performance data
  - Query CloudBees FM API for impressions
  - Collect rollout history
  - Pull application metrics
  - Gather user feedback

- [ ] **trend-analyzer** - Analyzes trends over time
  - Plot time series data
  - Calculate trend direction
  - Detect anomalies
  - Compare to baseline

- [ ] **segment-analyzer** - Breaks down by user segments
  - Group metrics by user attributes
  - Compare segment performance
  - Identify high/low performing segments
  - Analyze targeting effectiveness

- [ ] **performance-visualizer** - Creates charts and graphs
  - Generate time series plots
  - Create comparison charts
  - Format data tables
  - Highlight key insights

- [ ] **recommendation-engine** - Provides actionable insights
  - Synthesize findings
  - Generate recommendations
  - Calculate confidence levels
  - Suggest next steps

### Supporting Skills
- [ ] **anomaly-detector** - Identifies unusual patterns
- [ ] **baseline-comparator** - Compares to historical baseline
- [ ] **statistical-analyzer** - Performs statistical tests
- [ ] **export-formatter** - Exports reports in multiple formats

## Constraints & Guardrails

**Must NOT do:**
- Make recommendations without sufficient data
- Ignore anomalies or concerning trends
- Misrepresent statistical significance
- Skip baseline comparisons
- Recommend changes during incidents

**Must ALWAYS do:**
- Check data sufficiency before analysis
- Compare to baseline period
- Highlight anomalies
- Provide confidence levels
- Show supporting data
- Consider multiple metrics
- Acknowledge limitations in data

## Edge Cases

1. **Case:** Flag created recently (<7 days)
   **Handling:** Warn about limited data, focus on available metrics

2. **Case:** No impressions recorded
   **Handling:** Flag potential SDK issue, verify flag evaluation

3. **Case:** Rollout changed multiple times rapidly
   **Handling:** Segment analysis by rollout period

4. **Case:** External factors affect metrics (holiday, incident)
   **Handling:** Annotate timeline, adjust interpretation

5. **Case:** Metrics contradictory (latency up, conversion up)
   **Handling:** Deep dive into correlation, provide nuanced analysis

6. **Case:** Missing baseline data
   **Handling:** Use industry benchmarks or similar flags

7. **Case:** Segment data unavailable
   **Handling:** Provide aggregate analysis only, recommend adding segments

## Success Metrics
- [ ] Analysis completion time: <2 minutes
- [ ] Recommendation accuracy: 85%+
- [ ] Data visualization clarity: 4.5+ stars
- [ ] Actionable insights: 3+ per analysis
- [ ] User makes decision: 90% of analyses
- [ ] Decision confidence: "Data helped" (4+ stars)

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Works with: JOURNEY-11-flag-health-audit
- Informs: JOURNEY-08-gradual-rollout-planner, JOURNEY-09-rollout-impact-analyzer
- Supports: JOURNEY-10-flag-approval-request-assistant (with data)
