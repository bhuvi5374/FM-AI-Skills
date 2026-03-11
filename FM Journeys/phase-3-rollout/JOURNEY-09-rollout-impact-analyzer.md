# Journey: Rollout Impact Analyzer

## Overview
**Goal:** Predict and analyze the impact of changing a flag's rollout percentage before making the change, reducing risk and surprises.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "What will happen if I increase this flag to 50%?"
- "Show me the impact of rolling this out to 25% more users"
- "Is it safe to increase this flag's percentage?"
- "Analyze the blast radius of this rollout change"

**Success Criteria:**
- Estimated affected user count calculated
- Code paths impacted identified
- Test coverage verified
- Dependencies checked
- Performance impact estimated
- Risk assessment provided
- User makes informed decision

## User Persona
- **Role:** Developer, Product Manager, DevOps Engineer
- **Pain Point:**
  - Uncertain what happens when increasing rollout %
  - Can't visualize number of affected users
  - Don't know which code will execute more
  - Worried about hidden dependencies
  - Fear of performance degradation at scale
  - Need data to make confident decisions

- **Current Manual Process:**
  1. Guess impact based on intuition
  2. Calculate user numbers manually
  3. Hope nothing breaks
  4. React to issues after change

  Time: 10-20 minutes of uncertainty, potential issues

## Agentic Happy Path

### Step 1: Understand Planned Change
**What the agent does:**
- Identify current rollout percentage
- Ask about target percentage
- Calculate delta (users affected)
- Identify environments being changed
- Check if change is increase or rollback

**Context needed:**
- Flag current state
- Total user base per environment
- Historical rollout data

**Example:**
```
Agent: "You want to change feature_new_checkout_page
in Production?"

Current: 10% rollout (~10,000 users)
Target: 50% rollout
Delta: +40% (+40,000 additional users)

This is a significant increase. Let me analyze
the impact before you proceed.
```

### Step 2: Calculate User Impact
**What the agent does:**
- Calculate exact number of additional users
- Identify user segments affected
- Show geographic distribution
- Estimate traffic increase
- Check if target groups involved

**Context needed:**
- Total users per environment
- User segmentation data
- Traffic patterns
- Time of day considerations

**Example:**
```
User Impact Analysis:

Currently Affected Users: 10,000
After Change: 50,000
New Users: +40,000 (4x increase)

User Distribution:
- US: +20,000 users
- EU: +12,000 users
- APAC: +8,000 users

Target Groups:
✓ Current: beta_users only (10%)
→ After: beta_users + 40% random sample

Traffic Impact:
- Checkout page views: +400/hour
- API calls to checkout service: +2,000/hour
- Database queries: +5,000/hour

Time Consideration:
Current time: 2:00 PM EST (peak hours)
Recommendation: Wait until 10:00 PM EST (low traffic)
or proceed cautiously with monitoring
```

### Step 3: Analyze Code Path Impact
**What the agent does:**
- Show which code will execute more frequently
- Identify performance implications
- Check database query load
- Review API dependencies
- Flag any known performance issues in code path

**Context needed:**
- Code implementation of flag
- Performance profile of ON vs OFF paths
- Database query patterns
- External API calls

**Example:**
```
Code Path Analysis:

Flag ON Path (will run 4x more):

Critical Sections:
1. NewCheckoutPage component
   - 3 API calls per render
   - 2 database queries
   - Image loading (5 images, 200KB total)

2. processOrderV2() function
   - Stripe API call (external dependency)
   - Order validation (complex logic)
   - Email notification trigger

Performance Concerns:
⚠️  NewCheckoutPage makes 3 API calls
    Impact: +2,000 API calls/hour
    Current capacity: 10,000/hour
    New load: 4,000/hour (40% of capacity)
    Status: WITHIN LIMITS ✓

⚠️  Stripe API rate limit: 100/second
    Current: 20/second
    After change: 80/second
    Status: SAFE but watch closely

✓  Database can handle 5,000 additional queries/hour
```

### Step 4: Check Dependencies & Test Coverage
**What the agent does:**
- Identify flag dependencies
- Check if dependent flags at correct state
- Verify test coverage for affected code
- Review recent changes to code
- Check for open bugs in code path

**Context needed:**
- Flag dependency graph
- Test coverage data
- Recent commits
- Bug tracker integration

**Example:**
```
Dependency Analysis:

Flag Dependencies:
✓ feature_new_payment_processor: 100% (required)
  Status: READY

⚠️  feature_order_notifications: 10% (optional)
  Impact: Only 10% of new checkout users
  will get new notifications. Others get old.
  Action: Consider syncing percentages

Test Coverage:
✓ Unit tests: 87% coverage
✓ Integration tests: 12 tests passing
⚠️  E2E tests: 3/5 passing (2 failing)
  Failing tests are in old checkout, not new
  Status: ACCEPTABLE

Recent Changes:
- 2 days ago: Performance optimization (commit abc123)
- 1 week ago: Bug fix for edge case (commit def456)
- No changes in last 24 hours ✓

Open Bugs:
None in new checkout path ✓
```

### Step 5: Provide Risk Assessment & Recommendation
**What the agent does:**
- Calculate overall risk score
- Highlight concerns
- Provide go/no-go recommendation
- Suggest monitoring focus areas
- Offer alternative approaches if risky

**Context needed:**
- Risk calculation algorithm
- Historical incident data
- Current system health

**Example:**
```
Impact Assessment Summary:

Risk Level: MEDIUM 🟡

Factors:
✓ User impact: Significant but manageable (4x increase)
✓ Code path: Well tested, recent optimizations
⚠️  Dependencies: One flag out of sync (minor)
⚠️  Time: Peak hours (not ideal)
✓ Capacity: Within system limits
✓ Rollback: Instant (via flag)

Concerns:
1. Stripe API load increase (watch rate limits)
2. Peak hour timing (higher baseline traffic)

Recommendation: PROCEED WITH CAUTION

Action Plan:
1. ✓ Increase to 50% now OR
2. ⏰ Wait until 10:00 PM (off-peak) - RECOMMENDED
3. 👀 Monitor these metrics closely:
   - Stripe API response time
   - Checkout completion rate
   - Error logs
4. 🚨 Rollback if:
   - Stripe errors > 0.5%
   - Checkout conversion drops > 5%
   - Response time > 3 seconds

Ready to proceed?
```

## Required Skills

### Core Skills
- [ ] **rollout-delta-calculator** - Calculates user impact
  - Parse current and target percentages
  - Calculate affected user count
  - Determine traffic increase
  - Consider time-of-day factors

- [ ] **code-path-analyzer** - Analyzes code execution impact
  - Identify code paths for ON vs OFF
  - Analyze performance characteristics
  - Count API calls and database queries
  - Flag performance concerns

- [ ] **capacity-checker** - Validates system capacity
  - Check API rate limits
  - Verify database capacity
  - Review infrastructure limits
  - Estimate resource usage

- [ ] **dependency-validator** - Checks flag dependencies
  - Map flag dependency graph
  - Verify dependent flags in correct state
  - Check for circular dependencies
  - Warn about misaligned dependencies

- [ ] **test-coverage-analyzer** - Reviews test coverage
  - Check unit test coverage
  - Verify integration tests exist
  - Review E2E test results
  - Identify untested code paths

- [ ] **risk-scorer** - Calculates overall risk
  - Aggregate risk factors
  - Calculate risk score
  - Provide go/no-go recommendation
  - Suggest mitigation strategies

### Supporting Skills
- [ ] **traffic-pattern-analyzer** - Analyzes traffic patterns
- [ ] **performance-predictor** - Predicts performance impact
- [ ] **bug-tracker-checker** - Reviews open bugs
- [ ] **recent-change-analyzer** - Reviews recent code changes

## Constraints & Guardrails

**Must NOT do:**
- Recommend rollout without capacity check
- Ignore flag dependencies
- Skip test coverage review
- Proceed if critical dependencies missing
- Allow >2x increase without extra validation

**Must ALWAYS do:**
- Calculate exact user impact
- Check system capacity limits
- Verify dependent flags
- Review test coverage
- Provide risk score
- Suggest monitoring focus
- Define rollback criteria
- Consider time-of-day factors

## Edge Cases

1. **Case:** Rollout increase >50% in one step
   **Handling:** Flag high risk, recommend smaller increments

2. **Case:** No historical performance data
   **Handling:** Extra conservative, recommend 1-5% test first

3. **Case:** Critical dependency flag at 0%
   **Handling:** Block rollout, require dependency rollout first

4. **Case:** System already at 80% capacity
   **Handling:** Flag red, recommend infrastructure scaling first

5. **Case:** Rollout to 0% (full rollback)
   **Handling:** Simplified analysis, focus on reverting changes

6. **Case:** Multiple flags being changed simultaneously
   **Handling:** Analyze combined impact, warn about debugging difficulty

7. **Case:** Flag affects different user types differently
   **Handling:** Break down impact by user segment, provide detailed view

## Success Metrics
- [ ] Accurate user impact prediction (±5%)
- [ ] Zero surprise capacity issues
- [ ] Risk assessment accuracy: 90%+
- [ ] User confidence in decisions: 4.5+ stars
- [ ] Time to analyze: Under 2 minutes
- [ ] Issues caught before rollout: 95%+

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Works with: JOURNEY-08-gradual-rollout-planner
- Used before: Any flag percentage change
- Complements: JOURNEY-10-flag-approval-request-assistant
- Prevents issues in: JOURNEY-11-flag-health-audit
