# Journey: Flag Approval Request Assistant

## Overview
**Goal:** Streamline creation of comprehensive flag change approval requests with all necessary context for reviewers to make informed decisions quickly.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "I need approval to roll this flag to 100%"
- "Help me create an approval request for this flag change"
- "Generate a change request for production rollout"
- "What should I include in my flag approval request?"

**Success Criteria:**
- Comprehensive approval request created
- All relevant context included
- Impact analysis attached
- Appropriate reviewers identified
- Request submitted successfully
- Faster approval cycle

## User Persona
- **Role:** Developer, Product Manager
- **Pain Point:**
  - Approval requests get rejected for missing info
  - Unsure what context to include
  - Don't know who to request approval from
  - Requests sit in queue due to unclear info
  - Back-and-forth wastes time
  - Urgent changes delayed by approval process

- **Current Manual Process:**
  1. Write brief approval request
  2. Get rejected for missing details
  3. Add more info
  4. Wrong approver, restart process
  5. Finally approved after days

  Time: 1-3 days for approval, multiple iterations

## Agentic Happy Path

### Step 1: Gather Flag Change Details
**What the agent does:**
- Identify which flag needs approval
- Understand the requested change
- Determine environments affected
- Check if change is standard or exceptional
- Verify change is necessary

**Context needed:**
- Flag current state
- Proposed change
- Reason for change
- Urgency level

**Example:**
```
Agent: "Which flag needs approval?"
User: "feature_new_checkout_page, I want to roll to 100% in production"

Agent: "Current state: 50% in production
You want: 100% in production
Change: +50% rollout (additional 50,000 users)

Why now?
User: "Feature has been stable at 50% for a week, metrics good"

Agent: "Got it. Let me gather the evidence
for your approval request."
```

### Step 2: Collect Supporting Evidence
**What the agent does:**
- Pull flag performance metrics
- Show monitoring data from current rollout
- Gather test results
- Check Jira ticket status
- Review recent incidents
- Collect user feedback

**Context needed:**
- Monitoring system access
- Historical performance data
- Test coverage reports
- Jira integration
- Incident tracking

**Example:**
```
Evidence Collection:

Performance Metrics (Last 7 days at 50%):
✓ Error rate: 0.03% (baseline: 0.05%) - IMPROVED
✓ P95 latency: 1.2s (baseline: 1.5s) - IMPROVED
✓ Conversion rate: 4.8% (baseline: 4.5%) - IMPROVED
✓ Zero production incidents
✓ Support tickets: 2 total (minor UI questions)

User Feedback:
✓ 47 positive comments
✓ 3 neutral comments
✓ 0 negative comments

Test Coverage:
✓ Unit tests: 92% coverage, all passing
✓ Integration tests: 15 tests, all passing
✓ E2E tests: 8 scenarios, all passing

Jira Status:
✓ SHOP-456: Closed (Done)
✓ All acceptance criteria met
✓ Product sign-off received

This is strong evidence for approval!
```

### Step 3: Analyze Impact & Risk
**What the agent does:**
- Run impact analysis for proposed change
- Calculate additional affected users
- Identify risks and mitigation
- Check system capacity
- Define rollback plan

**Context needed:**
- Impact analysis from previous journey
- Risk assessment data
- Capacity information

**Example:**
```
Impact Analysis:

Additional Users Affected: 50,000 (+50%)
Total Production Users: 100,000 (100% after change)

Expected Impact:
- Additional API calls: +2,000/hour
- Database load: +5,000 queries/hour
- Infrastructure: Well within capacity ✓

Risk Assessment: LOW 🟢

Reasons:
✓ Stable at 50% for 7 days
✓ All metrics improved vs baseline
✓ No incidents during current rollout
✓ Strong test coverage
✓ Product approval received

Rollback Plan:
- Immediate: Set flag to 0% or 50%
- Recovery time: < 30 seconds
- On-call: Alice (Slack: @alice)

Recommended Approval: APPROVE
```

### Step 4: Identify Appropriate Reviewers
**What the agent does:**
- Determine approval requirements based on risk
- Identify required approvers by role
- Check approver availability
- Suggest alternate approvers if needed
- Set appropriate urgency level

**Context needed:**
- Approval workflow configuration
- Team structure and roles
- Approver on-call schedule
- Risk level of change

**Example:**
```
Approval Requirements:

Based on:
- Change type: Rollout increase to 100%
- Risk level: Low
- Environment: Production
- Impact: High user count

Required Approvers:
1. Engineering Lead (1 of):
   - Alice Chen (Primary, available)
   - Bob Smith (Backup, on vacation)
   → Assign to: Alice

2. Product Owner (1 of):
   - Carol Davis (available)
   → Assign to: Carol

Optional (FYI only):
- DevOps team (#devops channel)

Estimated approval time: 4-8 hours
Urgency: Normal (not blocking)
```

### Step 5: Generate & Submit Request
**What the agent does:**
- Create comprehensive approval document
- Format for readability
- Include all evidence
- Add visual charts if available
- Submit through proper channels
- Set up notifications

**Context needed:**
- Approval system integration
- Document templates
- Notification preferences

**Example:**
```
Approval Request Generated:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FLAG CHANGE APPROVAL REQUEST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

FLAG: feature_new_checkout_page
CHANGE: 50% → 100% in Production
REQUESTOR: John Doe
DATE: March 18, 2026

SUMMARY:
Request approval to complete rollout of new checkout
page to 100% of production users. Feature has been
stable at 50% for 7 days with improved metrics.

CURRENT STATE:
- Production: 50% rollout
- Affected users: 50,000
- Status: Stable, no incidents

PROPOSED CHANGE:
- Production: 100% rollout (+50%)
- Additional users: +50,000
- Total affected: 100,000 (all users)

EVIDENCE OF STABILITY:
✓ Error rate improved: 0.05% → 0.03%
✓ Latency improved: 1.5s → 1.2s
✓ Conversion improved: 4.5% → 4.8%
✓ Zero incidents in 7 days
✓ Positive user feedback (47 positive, 0 negative)

JIRA: SHOP-456 (Closed, product sign-off ✓)

IMPACT ANALYSIS:
- Risk level: LOW
- Infrastructure: Within capacity
- Traffic increase: Manageable (+2,000 calls/hour)

ROLLBACK PLAN:
- Method: Instant flag toggle to 0% or 50%
- Recovery time: < 30 seconds
- On-call: Alice (@alice in Slack)

TEST COVERAGE:
- Unit: 92%, all passing
- Integration: 15 tests passing
- E2E: 8 scenarios passing

RECOMMENDATION: APPROVE
This feature has demonstrated stability and
improved metrics. Risk is low with instant rollback
capability. Ready for full production rollout.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Submitted to:
✓ Alice Chen (Engineering Lead)
✓ Carol Davis (Product Owner)
ℹ️  #devops (FYI)

Notification sent via Slack ✓
Tracking: APR-2026-0318-001
```

## Required Skills

### Core Skills
- [ ] **change-details-collector** - Gathers change information
  - Identify flag and proposed change
  - Determine environments
  - Understand change rationale
  - Assess urgency

- [ ] **evidence-gatherer** - Collects supporting data
  - Pull performance metrics
  - Gather test results
  - Check Jira status
  - Collect user feedback
  - Review incident history

- [ ] **impact-analyzer** - Analyzes change impact
  - Run impact analysis
  - Calculate affected users
  - Assess risk level
  - Define rollback plan

- [ ] **approver-identifier** - Determines required approvers
  - Map change to approval requirements
  - Identify appropriate reviewers
  - Check approver availability
  - Set urgency level

- [ ] **approval-document-generator** - Creates request document
  - Format comprehensive approval request
  - Include all evidence
  - Add visual aids
  - Apply proper structure

- [ ] **approval-submitter** - Submits through proper channels
  - Submit to approval system
  - Notify reviewers
  - Set up status tracking
  - Handle submission errors

### Supporting Skills
- [ ] **approval-policy-checker** - Validates against approval policies
- [ ] **visual-chart-generator** - Creates performance charts
- [ ] **approval-tracker** - Tracks approval status
- [ ] **approval-reminder** - Sends reminders to reviewers

## Constraints & Guardrails

**Must NOT do:**
- Submit incomplete approval requests
- Skip impact analysis
- Choose wrong approvers
- Misrepresent risk level
- Omit rollback plan
- Submit during blackout periods without justification

**Must ALWAYS do:**
- Include all supporting evidence
- Provide accurate impact analysis
- Define clear rollback plan
- Identify correct approvers
- Set appropriate urgency
- Document test coverage
- Link to related tickets
- Follow approval policy

## Edge Cases

1. **Case:** Emergency change requiring immediate approval
   **Handling:** Fast-track process, reduced requirements, escalation path

2. **Case:** No performance data (brand new flag)
   **Handling:** Rely on test coverage, staging results, start with small %

3. **Case:** Previous approval for same flag was rejected
   **Handling:** Reference previous request, show what changed, address concerns

4. **Case:** Required approver unavailable (vacation, illness)
   **Handling:** Escalate to backup approver, document exception

5. **Case:** High-risk change requiring executive approval
   **Handling:** Add executive summary, schedule review meeting

6. **Case:** Conflicting approvals (engineering yes, product no)
   **Handling:** Escalate to joint decision maker, schedule discussion

7. **Case:** Approval needed outside business hours
   **Handling:** Follow on-call escalation process, justify urgency

## Success Metrics
- [ ] First-time approval rate: 90%+
- [ ] Approval time: <8 hours (from 1-3 days)
- [ ] Back-and-forth iterations: <1 per request
- [ ] Complete information included: 95%+
- [ ] Correct approvers identified: 100%
- [ ] Requester satisfaction: 4.5+ stars
- [ ] Approver satisfaction: "Easy to review" (4+ stars)

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Works with: JOURNEY-09-rollout-impact-analyzer (provides impact data)
- Follows: JOURNEY-08-gradual-rollout-planner (at phase gates)
- Enables: Faster, smoother approval cycles
- Precedes: Actual rollout execution
