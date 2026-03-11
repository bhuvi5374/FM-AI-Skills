# Journey: Flag Cleanup Assistant

## Overview
**Goal:** Identify and remove stale feature flags (100% rollout >14 days) along with dead code paths to reduce technical debt.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Help me identify and clean up old flags"
- "Which flags can I remove?"
- "Show me stale flags ready for cleanup"
- "Clean up feature_old_dashboard"

**Success Criteria:**
- Stale flags identified
- Dead code paths located
- Code safely removed (or guidance provided)
- Tests pass successfully
- Pull Request created (or manual steps provided)
- Flag deleted from FM system
- No production functionality broken

## User Persona
- **Role:** Software Developer, Tech Lead, DevOps Engineer
- **Pain Point:**
  - Feature flags accumulate over time
  - Dead code clutters codebase
  - Manual cleanup is tedious and risky
  - Hard to remember which flags are safe to remove
  - Cleaning one flag takes 30-60 minutes
  - Fear of breaking something

- **Current Manual Process:**
  1. Check flag dashboard to find 100% flags
  2. Calculate how long at 100%
  3. Search codebase for all flag references
  4. Manually identify dead code branches
  5. Carefully delete code
  6. Run tests
  7. Create PR
  8. Delete flag from FM

  Time: 30-60 minutes per flag

## Agentic Happy Path

### Step 1: Identify Cleanup Candidates
**What the agent does:**
- Query all flags from CloudBees FM
- Filter flags at 100% rollout
- Calculate days at 100% per environment
- Check all environments are stable
- Prioritize by age and usage

**Context needed:**
- CloudBees FM API access
- Flag rollout history
- Environment configuration
- Stability criteria (typically >14 days)

**Example:**
```
Scanning for cleanup candidates...

Found 18 flags eligible for cleanup:

Top Priority (100% for >90 days):
1. feature_old_dashboard (150 days at 100%)
   - All envs: 100% for >90 days
   - Code refs: 12 locations
   - Priority: HIGH

2. rollout_new_search (120 days at 100%)
   - All envs: 100% for 120 days
   - Code refs: 8 locations
   - Priority: HIGH

Medium Priority (100% for 30-90 days):
3. feature_checkout_v2 (45 days at 100%)
4. experiment_pricing_page (35 days at 100%)
...

Ready to analyze these for cleanup?
```

### Step 2: Analyze Code Impact
**What the agent does:**
- Search codebase for all flag references
- Identify if/else code blocks
- Determine which code is "active" vs "dead"
- Count lines of code to remove
- Check test files

**Context needed:**
- Code repository access
- Programming language
- Flag evaluation patterns
- Test framework

**Example:**
```
Analyzing: feature_old_dashboard

Code References Found: 12 locations

Active Code Path (flag is ON):
src/dashboard/Dashboard.tsx:
  Lines 45-78: NewDashboard component ✓ KEEP
  Lines 80-120: OldDashboard component ✗ DELETE

src/api/dashboard.ts:
  Lines 23-45: fetchNewDashboardData() ✓ KEEP
  Lines 47-89: fetchOldDashboardData() ✗ DELETE

tests/dashboard.test.ts:
  Lines 10-50: Tests for new dashboard ✓ KEEP
  Lines 52-98: Tests for old dashboard ✗ DELETE

Summary:
- Total files: 7
- Active code: Keep 234 lines
- Dead code: Delete 189 lines
- Tests to update: 3 files

Estimated cleanup time: 30 minutes
```

### Step 3: Remove Dead Code
**What the agent does:**
- Remove if/else conditional wrappers
- Delete dead code branches
- Keep only active code
- Update imports if needed
- Clean up test files
- Maintain code formatting

**Context needed:**
- Code modification permissions
- Language syntax rules
- Code formatting standards

**Example:**
```
Code Cleanup:

File: src/dashboard/Dashboard.tsx

BEFORE (Lines 40-125):
```typescript
function Dashboard() {
  const useNewDashboard = useFlag('feature_old_dashboard');

  if (useNewDashboard) {
    return <NewDashboard />; // ✓ Keep this
  } else {
    return <OldDashboard />; // ✗ Remove this
  }
}
```

AFTER (Lines 40-42):
```typescript
function Dashboard() {
  return <NewDashboard />;
}
```

Changes:
- Removed flag check (no longer needed)
- Removed conditional logic
- Deleted OldDashboard component (dead code)
- Simplified to just NewDashboard
- Cleaner, easier to understand

Apply this change? [Yes/No]
```

### Step 4: Validate & Test
**What the agent does:**
- Run test suite automatically
- Check for syntax errors
- Verify build succeeds
- Report test results
- Rollback if tests fail

**Context needed:**
- Test command
- CI/CD integration
- Build system

**Example:**
```
Running Tests...

Build Status:
✓ TypeScript compilation: Success
✓ Linting: No errors
✓ Build: Success

Test Results:
✓ Unit tests: 847/847 passing
✓ Integration tests: 23/23 passing
✓ E2E tests: 12/12 passing

Time: 2 minutes 34 seconds

All tests passing! ✓
Code cleanup is safe to commit.
```

### Step 5: Create PR & Delete Flag
**What the agent does:**
- Create git branch
- Commit changes with clear message
- Create Pull Request with details
- Remind to delete flag after PR merges
- Update team tracking

**Context needed:**
- Git repository access
- PR template
- Team notification preferences

**Example:**
```
Pull Request Created:

Branch: cleanup/feature-old-dashboard
Title: "Remove stale flag: feature_old_dashboard"

Description:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
## Flag Cleanup

**Flag:** feature_old_dashboard
**Status:** 100% rollout for 150 days (all environments)

## Changes
- Removed flag checks (7 files)
- Deleted OldDashboard component and dead code paths
- Cleaned up tests
- Deleted 189 lines of dead code
- Kept 234 lines of active code

## Testing
✓ All 882 tests passing
✓ Build successful
✓ No regressions detected

## Next Steps
After this PR merges:
1. Go to CloudBees FM Dashboard
2. Navigate to feature_old_dashboard flag
3. Click "Delete Flag"
4. Confirm deletion

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PR Link: https://github.com/yourorg/repo/pull/1234

IMPORTANT: Remember to delete flag from FM
after PR is merged to production!
```

## Required Skills

### Core Skills
- [ ] **stale-flag-detector** - Identifies cleanup candidates
  - Query all flags from FM
  - Filter by rollout percentage
  - Calculate days at 100%
  - Check environment stability
  - Prioritize cleanup order

- [ ] **code-reference-finder** - Locates all flag usage
  - Search codebase for flag references
  - Identify file and line numbers
  - Parse code structure
  - Categorize by file type (source, test)

- [ ] **dead-code-identifier** - Determines what to delete
  - Analyze if/else logic
  - Identify active vs dead paths
  - Count lines to remove
  - Check for nested conditions

- [ ] **code-remover** - Removes dead code safely
  - Delete conditional wrappers
  - Remove dead branches
  - Maintain formatting
  - Update imports
  - Clean up tests

- [ ] **test-validator** - Ensures cleanup is safe
  - Run test suite
  - Check build status
  - Verify no regressions
  - Report results

- [ ] **pr-creator** - Creates pull request
  - Create git branch
  - Commit changes
  - Generate PR description
  - Notify team

### Supporting Skills
- [ ] **multi-repo-scanner** - Handles flags across multiple repositories
- [ ] **flag-dependency-checker** - Verifies no dependent flags
- [ ] **cleanup-scheduler** - Plans bulk cleanup operations
- [ ] **flag-deleter-reminder** - Reminds to delete from FM after merge

## Constraints & Guardrails

**Must NOT do:**
- Remove flags younger than 14 days at 100%
- Delete code without running tests first
- Make changes directly to main branch
- Skip flags with active experiments
- Remove flags that are kill switches (0% with purpose)
- Delete flags during production incidents

**Must ALWAYS do:**
- Verify flag is 100% in ALL environments
- Confirm flag age >14 days at 100%
- Find ALL code references
- Run full test suite
- Create PR (never direct commit)
- Preserve code history in git
- Remind to delete flag from FM
- Check for flag dependencies

## Edge Cases

1. **Case:** Flag used in multiple repositories
   **Handling:** Identify all repos, create separate PRs, coordinate cleanup

2. **Case:** Flag checked multiple times in same file
   **Handling:** Find and clean ALL instances, not just first

3. **Case:** Dead code contains important comments/documentation
   **Handling:** Preserve valuable comments, move to active code if needed

4. **Case:** Flag controls large code block (>100 lines)
   **Handling:** Flag for human review, large changes need scrutiny

5. **Case:** Dead code branch has database migrations
   **Handling:** DO NOT auto-delete, flag for manual review

6. **Case:** Tests fail after cleanup
   **Handling:** Rollback changes, create issue for investigation

7. **Case:** Flag is parent/child of other flags
   **Handling:** Check dependencies, clean in correct order

## Success Metrics
- [ ] Cleanup time: <10 minutes per flag (vs 30-60 manual)
- [ ] Code correctly identified: 95%+
- [ ] Zero production incidents from cleanup
- [ ] All tests pass after cleanup: 100%
- [ ] Flags cleaned after audit: 50%+ within 30 days
- [ ] Developer satisfaction: 4.5+ stars
- [ ] Time saved: ~40 minutes per flag

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Triggered by: JOURNEY-11-flag-health-audit
- Works with: JOURNEY-16-dead-code-removal-assistant
- Follows: JOURNEY-17-orphaned-flag-detector
- Reduces: Technical debt over time
