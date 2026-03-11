# Journey: Dead Code Removal Assistant

## Overview
**Goal:** Help users safely remove dead code branches when a specific flag is permanently ON or OFF, without the full cleanup workflow.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "This flag is fully rolled out, help me remove the dead code"
- "feature_checkout is permanent ON, clean up the else branch"
- "Show me which code is dead for this flag"
- "Guide me through removing unused code paths"

**Success Criteria:**
- Dead code branches identified
- User understands which code to remove
- Code cleanup guidance provided
- Safe removal process followed
- Tests validated after cleanup
- Production functionality intact

## User Persona
- **Role:** Developer
- **Pain Point:**
  - Flag is permanent but code still has conditionals
  - Unsure which branch is "dead"
  - Want to simplify code without breaking it
  - Need guidance on safe removal
  - Don't want full automated cleanup, prefer manual control

- **Current Manual Process:**
  1. Look at flag status
  2. Guess which code branch never runs
  3. Carefully delete code
  4. Hope nothing breaks
  5. Fix issues if tests fail

  Time: 15-30 minutes, error-prone

## Agentic Happy Path

### Step 1: Understand Flag State
**What the agent does:**
- Ask which flag to analyze
- Check flag current state (ON/OFF/percentage)
- Verify flag is permanent (not temporary)
- Determine which code path is active

**Context needed:**
- Flag name
- Flag rollout state
- Flag purpose (permanent vs temporary)

**Example:**
```
Agent: "Which flag are you cleaning up?"
User: "feature_new_checkout_page"

Agent: "Checking flag status...
- Production: 100% (ON)
- Stable for: 45 days
- Type: Permanent feature rollout

When flag is ON:
- 'if' branch runs (ACTIVE) ✓
- 'else' branch never runs (DEAD) ✗

The 'else' branch is safe to remove.
Let me find all instances."
```

### Step 2: Find & Explain Code Paths
**What the agent does:**
- Search codebase for flag references
- Show each if/else block
- Mark which branch is dead
- Explain the logic clearly
- Provide visual examples

**Context needed:**
- Code repository access
- Flag name patterns
- Programming language

**Example:**
```
Found 5 code locations:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Location 1: src/checkout/CheckoutPage.tsx:45
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CURRENT CODE:
```typescript
if (isFeatureEnabled('feature_new_checkout_page')) {
  return <NewCheckoutPage />; // ✓ ACTIVE (flag ON)
} else {
  return <OldCheckoutPage />; // ✗ DEAD (never runs)
}
```

EXPLANATION:
Since flag is 100% ON:
- Top branch ALWAYS runs
- Bottom branch NEVER runs

AFTER CLEANUP:
```typescript
return <NewCheckoutPage />;
```

Remove:
- The if/else wrapper
- The entire else block (OldCheckoutPage)
Keep:
- Just the active code (NewCheckoutPage)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Location 2: src/api/checkout.ts:89
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CURRENT CODE:
```typescript
const endpoint = isFeatureEnabled('feature_new_checkout_page')
  ? '/api/v2/checkout' // ✓ ACTIVE
  : '/api/v1/checkout'; // ✗ DEAD
```

AFTER CLEANUP:
```typescript
const endpoint = '/api/v2/checkout';
```

Similar pattern - remove ternary, keep active value.
```

### Step 3: Provide Removal Checklist
**What the agent does:**
- Generate step-by-step cleanup guide
- Prioritize by risk (start with safest)
- Show before/after for each change
- Provide testing guidance
- Warn about tricky cases

**Context needed:**
- Code complexity
- Testing requirements
- Risk assessment

**Example:**
```
Cleanup Checklist:

STEP 1: Test Files (Lowest Risk - Start Here)
□ tests/checkout.test.ts:67-89
  Remove tests for old checkout (23 lines)
  Keep tests for new checkout

STEP 2: API Utilities (Low Risk)
□ src/api/checkout.ts:89
  Change ternary to direct assignment

□ src/api/checkout.ts:123
  Remove old API response parsing

STEP 3: Components (Medium Risk)
□ src/checkout/CheckoutPage.tsx:45-78
  Remove if/else, keep NewCheckoutPage
  Delete OldCheckoutPage import

□ src/checkout/Payment.tsx:102-145
  Remove old payment flow

STEP 4: Validation (Do This After Each Step)
□ Run: npm test
□ Check: All tests passing
□ Build: npm run build

TRICKY CASE:
⚠️  src/checkout/shared-utils.ts:234
This file has NESTED conditionals:
- Outer if: feature_new_checkout_page
- Inner if: another_flag

Be extra careful! Remove only outer if/else.
Preserve inner logic.
```

### Step 4: Guide Manual Cleanup
**What the agent does:**
- Walk user through each change
- Answer questions during cleanup
- Provide copy-paste examples
- Help with edge cases
- Verify understanding

**Context needed:**
- User's progress
- Questions raised
- IDE/editor being used

**Example:**
```
Agent: "Ready to start? Let's go file by file."

User: "Yes, starting with tests"

Agent: "Great! Open tests/checkout.test.ts
Go to lines 67-89.

You should see:
  describe('Old checkout', () => {
    // 23 lines of tests
  });

Simply DELETE those 23 lines.
Keep the 'New checkout' tests above.

Done? [Yes/No]"

User: "Done"

Agent: "Perfect! Now run: npm test
Do all tests pass?"

User: "Yes, 847/847 passing"

Agent: "Excellent! ✓ Tests validated.
Next file: src/api/checkout.ts line 89..."
```

### Step 5: Validate & Document
**What the agent does:**
- Verify all cleanup complete
- Run final test suite
- Generate cleanup summary
- Update documentation
- Remind about flag deletion

**Context needed:**
- Cleanup completion status
- Test results
- Documentation location

**Example:**
```
Cleanup Summary:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
COMPLETED CLEANUP: feature_new_checkout_page
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Files Modified: 5
- tests/checkout.test.ts ✓
- src/api/checkout.ts ✓
- src/checkout/CheckoutPage.tsx ✓
- src/checkout/Payment.tsx ✓
- src/checkout/shared-utils.ts ✓

Code Removed:
- Dead code: 167 lines deleted
- Flag checks: 5 conditionals removed
- Imports: 3 unused imports removed

Tests: ✓ All 847 passing

Next Steps:
1. Create PR with your changes
   Suggested title: "Remove dead code for feature_new_checkout_page"

2. After PR merges:
   - Delete flag from CloudBees FM dashboard
   - Update team documentation

3. Celebrate! You reduced technical debt! 🎉

Estimated time saved in future:
- Developer cognitive load: Reduced
- Code complexity: Simplified
- Future maintenance: Easier
```

## Required Skills

### Core Skills
- [ ] **flag-state-analyzer** - Determines flag permanence
  - Check flag current state
  - Verify stability period
  - Determine active vs dead paths
  - Confirm permanent vs temporary

- [ ] **code-path-explainer** - Explains which code is dead
  - Find all flag references
  - Parse if/else logic
  - Identify dead branches
  - Provide clear visual examples

- [ ] **cleanup-guide-generator** - Creates step-by-step guide
  - Prioritize cleanup order
  - Generate before/after examples
  - Flag tricky cases
  - Provide testing checkpoints

- [ ] **interactive-cleanup-coach** - Guides manual cleanup
  - Walk through each file
  - Answer questions
  - Provide copy-paste examples
  - Validate progress

- [ ] **cleanup-validator** - Verifies completion
  - Check all references cleaned
  - Run final tests
  - Generate summary
  - Provide next steps

### Supporting Skills
- [ ] **nested-conditional-handler** - Handles complex nested flags
- [ ] **import-cleanup-helper** - Removes unused imports
- [ ] **doc-updater** - Updates related documentation
- [ ] **git-commit-suggester** - Suggests good commit messages

## Constraints & Guardrails

**Must NOT do:**
- Recommend cleanup if flag not stable (< 14 days)
- Auto-delete without user confirmation
- Skip tricky cases without warning
- Proceed if tests fail
- Remove code without clear explanation

**Must ALWAYS do:**
- Verify flag is permanent (stable)
- Show before/after for each change
- Prioritize low-risk changes first
- Validate with tests after each step
- Warn about nested conditionals
- Explain the "why" not just "what"
- Remind about flag deletion from FM

## Edge Cases

1. **Case:** Nested conditionals with multiple flags
   **Handling:** Show careful step-by-step, flag as tricky, recommend manual review

2. **Case:** Dead code contains important comments
   **Handling:** Suggest preserving comments, move to active code

3. **Case:** Flag checked in multiple ways (if, ternary, switch)
   **Handling:** Show examples for each pattern type

4. **Case:** Flag is OFF (not ON) - inverse logic
   **Handling:** Flip explanation, 'if' branch is dead, 'else' is active

5. **Case:** Flag at partial rollout, not 100%
   **Handling:** Explain NOT safe to remove yet, wait for 100%

6. **Case:** User unsure about a section
   **Handling:** Offer to skip, mark for later review, get second opinion

7. **Case:** Code has side effects in dead branch
   **Handling:** Deep analysis, potential issues, recommend testing

## Success Metrics
- [ ] User successfully removes dead code: 90%+
- [ ] Tests pass after cleanup: 100%
- [ ] Zero production incidents: 100%
- [ ] Time to clean: 15-20 minutes (vs 30-60 manual)
- [ ] User confidence: "Understood what to do" (4.5+ stars)
- [ ] Completion rate: 85%+ finish cleanup

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Complements: JOURNEY-15-flag-cleanup-assistant (manual vs automated)
- Works with: JOURNEY-07-flag-testing-assistant (testing guidance)
- Part of: Overall technical debt reduction strategy
