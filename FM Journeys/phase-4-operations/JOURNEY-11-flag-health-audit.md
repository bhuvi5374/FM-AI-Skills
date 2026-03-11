# Journey: Flag Health Audit

## Overview
**Goal:** Generate comprehensive health report for all feature flags, identifying cleanup candidates, unused flags, and flags needing attention.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Show me the health of all my feature flags"
- "Which flags need cleanup?"
- "Audit all my flags"
- "Generate flag health report"

**Success Criteria:**
- Complete inventory of all flags
- Health status assigned to each flag
- Cleanup candidates identified
- Action items generated
- Report shared with team
- User understands next steps

## User Persona
- **Role:** Engineering Manager, Tech Lead, DevOps Engineer, Product Manager
- **Pain Point:**
  - Flags accumulate without oversight
  - Unsure which flags are stale
  - No visibility into overall flag portfolio
  - Don't know where to start with cleanup
  - Need periodic health checks
  - Want to prevent technical debt

- **Current Manual Process:**
  1. Manually check each flag status
  2. Guess which flags are old/unused
  3. Ask team if flags still needed
  4. Create spreadsheet to track cleanup

  Time: 2-4 hours, incomplete analysis

## Agentic Happy Path

### Step 1: Inventory All Flags
**What the agent does:**
- Query all flags from CloudBees FM
- Count total flags per application
- Group by environment
- Categorize by type (boolean, string, number)
- Show creation dates

**Context needed:**
- Access to all flags in system
- Multi-environment data
- Flag metadata

**Example:**
```
Flag Inventory:

Total Flags: 127
By Type:
- Boolean: 98 (77%)
- String: 21 (17%)
- Number: 8 (6%)

By Age:
- < 30 days: 12 flags (new)
- 30-90 days: 23 flags (active development)
- 90-180 days: 34 flags (mature)
- > 180 days: 58 flags (old, review needed)

By Environment Status:
- All envs at 100%: 42 flags (cleanup candidates)
- Partial rollout: 31 flags (in progress)
- All envs at 0%: 8 flags (unused?)
- Mixed: 46 flags (complex)
```

### Step 2: Analyze Flag Health
**What the agent does:**
- Check rollout percentage per environment
- Calculate "days at 100%" for each flag
- Check impression counts (usage)
- Identify flags with zero impressions
- Check last modification date
- Review code references

**Context needed:**
- Flag rollout history
- Impression data
- Code scanning results
- Modification audit log

**Example:**
```
Health Analysis:

HEALTHY FLAGS (35 flags)
✓ Active development (< 30 days old)
✓ Partial rollout with monitoring
✓ Regular impressions
✓ Recent modifications

Example: feature_dark_mode
- Created: 15 days ago
- Production: 25% rollout
- Impressions: 12,000/day
- Status: HEALTHY - Active rollout

STALE FLAGS (42 flags)
⚠️  100% rollout for > 14 days
⚠️  Ready for cleanup
⚠️  Code can be simplified

Example: feature_new_checkout_page
- Created: 120 days ago
- Production: 100% for 30 days
- Code refs: 5 locations
- Status: STALE - Cleanup recommended

UNUSED FLAGS (8 flags)
🚨 Zero or very low impressions
🚨 Possibly orphaned
🚨 Investigate before deleting

Example: experiment_pricing_test_v2
- Created: 60 days ago
- Production: 0% rollout
- Impressions: 0 (last 30 days)
- Status: UNUSED - Verify with team

ATTENTION NEEDED (42 flags)
📋 Stuck at partial rollout (>30 days)
📋 No documentation
📋 Conflicting env settings

Example: rollout_new_dashboard
- Created: 90 days ago
- Production: 50% for 45 days (stuck?)
- No Jira link
- Status: NEEDS ATTENTION - Review rollout plan
```

### Step 3: Generate Action Items
**What the agent does:**
- Prioritize flags by cleanup urgency
- Create action items per flag category
- Assign rough effort estimates
- Suggest ownership
- Generate cleanup roadmap

**Context needed:**
- Team structure
- Cleanup priorities
- Effort estimation

**Example:**
```
Recommended Actions:

PRIORITY 1: Quick Wins (High Value, Low Effort)
□ Clean up 12 flags at 100% for >90 days
  - Estimated time: 2 hours per flag
  - Total: ~24 hours (3 days)
  - Owner: Dev team
  - Impact: -12 flags, cleaner codebase

PRIORITY 2: Investigate Unused (Medium Value, Low Effort)
□ Review 8 flags with zero impressions
  - Verify with product team
  - Either activate or delete
  - Estimated time: 30 min per flag
  - Total: ~4 hours
  - Owner: Product + Dev

PRIORITY 3: Resolve Stuck Rollouts (High Value, Medium Effort)
□ Investigate 15 flags stuck at partial rollout
  - Determine if rollout should continue
  - Complete or rollback
  - Estimated time: 1 hour per flag
  - Total: ~15 hours (2 days)
  - Owner: Dev + Product

PRIORITY 4: Documentation Cleanup (Medium Value, Low Effort)
□ Add documentation to 27 undocumented flags
  - Add descriptions
  - Link Jira tickets
  - Estimated time: 15 min per flag
  - Total: ~7 hours (1 day)
  - Owner: Whoever created flag

PRIORITY 5: Long-term Maintenance (Low Urgency)
□ Set up monthly flag health reviews
□ Create flag lifecycle policy
□ Automate stale flag detection
```

### Step 4: Create Detailed Report
**What the agent does:**
- Generate comprehensive health report
- Include visualizations (charts, graphs)
- Add flag-by-flag details
- Provide executive summary
- Format for sharing

**Context needed:**
- Report templates
- Visualization capabilities
- Team communication preferences

**Example:**
```
FEATURE FLAG HEALTH REPORT
Generated: March 18, 2026
Scope: All applications, all environments

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
EXECUTIVE SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Total Flags: 127

Health Distribution:
✓ Healthy: 35 (28%)
⚠️  Stale: 42 (33%) ← Cleanup opportunity
🚨 Unused: 8 (6%)
📋 Needs Attention: 42 (33%)

Key Findings:
1. 42 flags ready for immediate cleanup
   - All at 100% for >14 days
   - ~80 hours of cleanup work
   - Will simplify codebase significantly

2. 8 unused flags to investigate
   - Zero impressions in last 30 days
   - May be orphaned or misconfigured

3. 15 flags stuck at partial rollout
   - Need product decision to proceed or rollback

Recommended Focus:
Start with Priority 1 (12 oldest flags at 100%)
Expected completion: 2 weeks with 1 engineer

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DETAILED FINDINGS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Flag-by-flag breakdown with recommendations]

Full report: [Export as PDF] [Export as CSV]
```

### Step 5: Schedule & Track Cleanup
**What the agent does:**
- Create tracking board/tickets
- Set up recurring health audits
- Assign ownership
- Set deadlines
- Create reminders

**Context needed:**
- Project management tools (Jira, etc.)
- Team capacity
- Cleanup timeline preferences

**Example:**
```
Cleanup Tracking:

Created Jira Epic: "Q1 2026 Flag Cleanup"
- 42 sub-tasks for stale flags
- Assigned to: Dev team rotation
- Target: Complete by March 31, 2026

Recurring Health Audit:
✓ Scheduled: Monthly, first Monday
✓ Calendar invite sent to: engineering@company.com
✓ Report auto-generated and shared in #flag-health

Metrics to Track:
- Total flag count (target: -40 by end of Q1)
- Stale flag percentage (target: <10%)
- Average flag age (target: <90 days)
- Flags at 100% for >30 days (target: 0)

Next Review: April 1, 2026
```

## Required Skills

### Core Skills
- [ ] **flag-inventory-collector** - Gathers all flags
  - Query CloudBees FM API for all flags
  - Collect metadata (type, age, creator)
  - Group by categories
  - Generate statistics

- [ ] **flag-health-analyzer** - Analyzes each flag
  - Check rollout percentages
  - Calculate days at 100%
  - Review impression data
  - Check code references
  - Assign health status

- [ ] **cleanup-prioritizer** - Creates action plan
  - Categorize flags by cleanup urgency
  - Estimate effort per flag
  - Generate prioritized task list
  - Suggest ownership

- [ ] **health-report-generator** - Creates comprehensive report
  - Format report with visualizations
  - Generate executive summary
  - Include flag-by-flag details
  - Export in multiple formats

- [ ] **cleanup-tracker** - Sets up tracking and monitoring
  - Create tracking tickets
  - Schedule recurring audits
  - Set up metrics dashboard
  - Send reminders

### Supporting Skills
- [ ] **visualization-generator** - Creates charts and graphs
- [ ] **trend-analyzer** - Tracks health trends over time
- [ ] **benchmark-comparator** - Compares against industry standards
- [ ] **policy-recommender** - Suggests flag lifecycle policies

## Constraints & Guardrails

**Must NOT do:**
- Auto-delete flags without team review
- Recommend cleanup of recently modified flags
- Skip flags without thorough analysis
- Make deletion decisions for team
- Ignore flags with active experiments

**Must ALWAYS do:**
- Analyze ALL flags in system
- Provide clear health criteria
- Show supporting evidence
- Prioritize recommendations
- Allow team to review before action
- Schedule follow-up audits
- Track cleanup progress

## Edge Cases

1. **Case:** Flag at 100% but recently rolled out (<7 days)
   **Handling:** Mark as "Monitoring" not "Stale", wait for stability

2. **Case:** Flag with zero impressions but intentional (kill switch)
   **Handling:** Identify operational flags, exclude from unused category

3. **Case:** Hundreds of flags (large enterprise)
   **Handling:** Paginate results, focus on highest priority subset first

4. **Case:** Flags managed by different teams
   **Handling:** Group by ownership, send team-specific reports

5. **Case:** Flags in multiple applications
   **Handling:** Provide per-app and consolidated views

6. **Case:** Conflicting environments (prod 100%, dev 0%)
   **Handling:** Flag for investigation, may be intentional or issue

7. **Case:** No code references found but flag is active
   **Handling:** May be server-side, check SDK usage patterns

## Success Metrics
- [ ] Audit completion time: <10 minutes for 100 flags
- [ ] Cleanup recommendations accuracy: 90%+
- [ ] Flags cleaned up after audit: 50%+ within 30 days
- [ ] False positives: <5%
- [ ] Team adopts regular audits: Monthly cadence
- [ ] User satisfaction: 4+ stars
- [ ] Technical debt reduction: 30%+ flags removed quarterly

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Enables: JOURNEY-15-flag-cleanup-assistant
- Complements: JOURNEY-12-flag-performance-analyzer
- Provides input for: JOURNEY-14-flag-documentation-generator
- Best practice: Run monthly or quarterly
