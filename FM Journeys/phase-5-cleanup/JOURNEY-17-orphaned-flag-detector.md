# Journey: Orphaned Flag Detector

## Overview
**Goal:** Identify flags that exist in CloudBees FM but have no code references, indicating they can be safely deleted from the system.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Find flags that exist in FM but not in code"
- "Which flags are orphaned?"
- "Show me flags I can delete from the dashboard"
- "Clean up flags with no code usage"

**Success Criteria:**
- Orphaned flags identified
- Zero code references confirmed
- Safe to delete flags listed
- Flags deleted from FM system
- Dashboard decluttered
- No functional impact

## User Persona
- **Role:** Engineering Manager, DevOps Engineer, Tech Lead
- **Pain Point:**
  - FM dashboard cluttered with old flags
  - Flags exist but code was already removed
  - Unsure which flags are truly unused
  - Fear of deleting needed flags
  - Want clean dashboard for team clarity
  - Need periodic FM cleanup

- **Current Manual Process:**
  1. Look at flags in FM dashboard
  2. Search codebase manually for each flag
  3. Guess if flag is needed
  4. Hesitate to delete (uncertainty)
  5. Leave orphaned flags forever

  Time: Never gets done, clutter accumulates

## Agentic Happy Path

### Step 1: Scan All Flags
**What the agent does:**
- Query all flags from CloudBees FM
- Get flag metadata (name, type, age, status)
- Count total flags
- Group by application/workspace

**Context needed:**
- CloudBees FM API access
- All flags across environments
- Flag metadata

**Example:**
```
Scanning CloudBees FM...

Total Flags: 127
- Active (used recently): 89
- Potentially orphaned: 38
- Need investigation: 38

Starting deep code scan to verify...
```

### Step 2: Search Codebase for References
**What the agent does:**
- Search all code repositories for flag names
- Check multiple reference patterns
- Include tests, configs, documentation
- Verify no dynamic flag references
- Cross-reference with SDK logs

**Context needed:**
- Code repository access
- Multiple repo support
- Flag naming patterns
- SDK implementation patterns

**Example:**
```
Code Scan Results:

Checking 38 potentially orphaned flags...

CONFIRMED ORPHANED (10 flags):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. feature_old_navigation
   - Created: 180 days ago
   - Status: 100% in all environments
   - Code references: NONE ✓
   - Last modified: 90 days ago
   - Safe to delete: YES

2. experiment_pricing_v1
   - Created: 240 days ago
   - Status: 0% in all environments
   - Code references: NONE ✓
   - Last modified: 180 days ago
   - Safe to delete: YES

3. rollout_beta_dashboard
   - Created: 300 days ago
   - Status: 100% in all environments
   - Code references: NONE ✓
   - Last modified: 120 days ago
   - Safe to delete: YES

... (7 more)

NOT ORPHANED (28 flags):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
These flags have active code references. ✓
```

### Step 3: Verify Orphan Status
**What the agent does:**
- Double-check with multiple search patterns
- Check impression data (SDK evaluations)
- Review audit logs
- Confirm flag not in dynamic references
- Validate safe to delete

**Context needed:**
- Impression/evaluation data
- Audit log access
- Dynamic reference patterns
- Safety verification criteria

**Example:**
```
Verification: feature_old_navigation

Code Search:
✓ Grep search: No matches
✓ IDE search: No matches
✓ Comment search: No matches
✓ Config files: No matches
✓ Documentation: No mentions

Impression Data (Last 90 days):
- SDK evaluations: 0 ✓
- No active usage detected

Audit Log:
- Last modified: 90 days ago
- Action: Set to 100% in production
- No changes since

Dynamic Reference Check:
- No getFlag() with variables
- No flag name concatenation detected
- Flag name is literal in all patterns

Deletion Risk: VERY LOW ✓

VERDICT: This flag is truly orphaned.
Code was removed but flag remains in FM.
Safe to delete from dashboard.
```

### Step 4: Categorize & Explain
**What the agent does:**
- Group orphaned flags by category
- Explain likely reason for orphaning
- Assess deletion priority
- Provide context for each flag
- Show deletion impact (none expected)

**Context needed:**
- Flag history
- Git history
- Team practices
- Business context

**Example:**
```
Orphan Categories:

CATEGORY 1: Code Cleaned Up, Flag Forgotten (7 flags)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
These flags reached 100%, code was removed,
but flag was never deleted from FM.

- feature_old_navigation (180 days old)
- feature_legacy_api (200 days old)
- rollout_beta_dashboard (300 days old)
...

Why orphaned: Cleanup process incomplete
Priority: HIGH (safe to delete immediately)

CATEGORY 2: Experiments Never Launched (2 flags)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Created for experiments that were cancelled
before code was written.

- experiment_pricing_v1 (240 days old)
- experiment_homepage_variant (150 days old)

Why orphaned: Feature cancelled
Priority: MEDIUM (clean up to reduce clutter)

CATEGORY 3: Test/Temporary Flags (1 flag)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Created for testing, never used in production.

- test_feature_toggle_demo (90 days old)

Why orphaned: Testing only
Priority: LOW (but good to clean up)
```

### Step 5: Safe Deletion Process
**What the agent does:**
- Provide deletion recommendations
- Generate deletion plan
- Create backup/archive
- Execute deletions (or guide manual)
- Confirm cleanup complete

**Context needed:**
- FM deletion API
- Backup requirements
- Team approval process
- Audit trail

**Example:**
```
Deletion Plan:

STEP 1: Export Flag Data (Backup)
Creating backup of 10 orphaned flags...
✓ Exported to: orphaned-flags-2026-03-18.json
✓ Backup stored in: /backups/flags/

STEP 2: Team Notification
Sending notification to #feature-flags:
"Planning to delete 10 orphaned flags.
Review list: [Link]
Speak up if any concerns by EOD."

STEP 3: Waiting Period (24 hours)
Giving team time to review...

STEP 4: Deletion (After Approval)

Deleting flags from CloudBees FM:

Priority 1: High Priority (7 flags)
1. feature_old_navigation
   - Deleted from: All environments ✓
   - Audit log: Recorded ✓

2. feature_legacy_api
   - Deleted from: All environments ✓
   - Audit log: Recorded ✓

... (5 more)

Priority 2: Medium Priority (2 flags)
... (experiments)

Priority 3: Low Priority (1 flag)
... (test flag)

All 10 flags deleted successfully! ✓

CLEANUP SUMMARY:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
- Flags deleted: 10
- Backup created: ✓
- Team notified: ✓
- Audit trail: ✓

Dashboard is now cleaner!
Total flags: 127 → 117 (-10)

Recommendation: Run orphan detection
monthly to prevent future clutter.
```

## Required Skills

### Core Skills
- [ ] **fm-flag-lister** - Lists all flags from FM
  - Query CloudBees FM API
  - Get all flag metadata
  - Group by workspace/app
  - Count totals

- [ ] **code-reference-scanner** - Searches for flag usage
  - Search all repositories
  - Multiple search patterns
  - Include tests and docs
  - Handle dynamic references

- [ ] **orphan-verifier** - Confirms orphan status
  - Double-check with multiple methods
  - Check impression data
  - Review audit logs
  - Assess deletion risk

- [ ] **orphan-categorizer** - Groups and explains
  - Categorize by orphan reason
  - Provide historical context
  - Assess deletion priority
  - Generate explanations

- [ ] **flag-deleter** - Removes from FM system
  - Create backup/export
  - Delete via FM API
  - Record audit trail
  - Confirm completion

### Supporting Skills
- [ ] **backup-creator** - Exports flag data before deletion
- [ ] **team-notifier** - Alerts team of planned deletions
- [ ] **approval-workflow** - Manages deletion approvals
- [ ] **audit-logger** - Records all deletions

## Constraints & Guardrails

**Must NOT do:**
- Delete flags without verification
- Skip backup before deletion
- Delete flags with ANY code references
- Delete flags with recent impressions (< 30 days)
- Delete without team notification
- Delete flags during incidents

**Must ALWAYS do:**
- Scan ALL repositories
- Check impression data
- Create backup before deletion
- Notify team of planned deletions
- Wait for review period
- Record deletion in audit log
- Verify deletion completed
- Consider false positives

## Edge Cases

1. **Case:** Flag name is substring of another flag
   **Handling:** Use exact match search, verify boundaries

2. **Case:** Flag used dynamically (computed name)
   **Handling:** Search for partial matches, flag as risky, manual review

3. **Case:** Flag exists in comments only
   **Handling:** Consider orphaned but note in documentation

4. **Case:** Flag in archived/legacy repository
   **Handling:** Include archived repos in scan, flag for review

5. **Case:** Zero impressions but flag is kill switch
   **Handling:** Identify operational flags, exclude from orphan list

6. **Case:** Flag created recently (<30 days)
   **Handling:** Exclude from orphan detection, too new

7. **Case:** Flag used in external system (not in codebase)
   **Handling:** Check FM API usage logs, third-party integrations

## Success Metrics
- [ ] Orphan detection accuracy: 95%+
- [ ] False positives: <2%
- [ ] Scan completion time: <5 minutes for 100 flags
- [ ] Flags deleted after detection: 80%+ within 7 days
- [ ] Zero incidents from deletions: 100%
- [ ] Dashboard clutter reduction: 10-20% of total flags
- [ ] Team satisfaction: "Dashboard cleaner" (4+ stars)

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Complements: JOURNEY-11-flag-health-audit
- Follows: JOURNEY-15-flag-cleanup-assistant (code cleaned, now FM cleanup)
- Part of: Ongoing FM maintenance
- Run: Monthly or quarterly
