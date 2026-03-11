# Journey: Flag Testing Assistant

## Overview
**Goal:** Guide developers to thoroughly test feature flags locally before deploying, ensuring both ON and OFF states work correctly and edge cases are covered.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "How do I test this flag locally?"
- "Help me test both ON and OFF states"
- "I need to test my flag before deploying"
- "What test cases should I write for this flag?"

**Success Criteria:**
- Flag tested in both ON and OFF states locally
- Edge cases identified and tested
- Test cases written for automation
- User understands local testing techniques
- Confidence flag works correctly before deployment

## User Persona
- **Role:** Developer, QA Engineer
- **Pain Point:**
  - Forget to test OFF state (only test new feature)
  - Unclear how to override flags locally
  - Don't know what edge cases to test
  - Fear of breaking production with untested flags
  - Need to test different target group scenarios
  - Unsure how to write automated flag tests

- **Current Manual Process:**
  1. Test feature with flag ON
  2. Forget to test OFF state
  3. Deploy to production
  4. Discover bugs when rolling back flag

  Time: Incomplete testing leads to production issues

## Agentic Happy Path

### Step 1: Understand Flag Being Tested
**What the agent does:**
- Identify which flag needs testing
- Understand flag's purpose and impact
- Determine both code paths (ON vs OFF)
- Identify critical user flows affected

**Context needed:**
- Flag name and type
- Code implementation
- User-facing impact
- Critical vs non-critical feature

**Example:**
```
Agent: "Which flag are you testing?"
User: "feature_new_checkout_page"

Agent: "Let me check your implementation...
I see this flag controls:
- ON: Shows new checkout UI
- OFF: Shows old checkout UI

Critical flows to test:
1. Complete checkout (both versions)
2. Error handling (invalid payment)
3. Edge cases (empty cart, expired session)

Let's test both paths thoroughly."
```

### Step 2: Set Up Local Testing Environment
**What the agent does:**
- Guide SDK local override configuration
- Show how to toggle flag without FM UI
- Explain local testing modes
- Set up test user contexts
- Configure test data

**Context needed:**
- Programming language/SDK
- Local development setup
- SDK testing capabilities

**Example:**
```
Local Testing Setup:

Option 1: Environment Variable Override
export FEATURE_NEW_CHECKOUT_PAGE=true
npm run dev

Option 2: SDK Testing Mode
import { Rox } from 'rox-browser';
Rox.overrides.setOverride('feature_new_checkout_page', true);

Option 3: FM Dashboard (Local Environment)
FM UI → Environments → Local → Set to ON/OFF

Recommended: Use Option 2 (SDK override) for
quick testing and automated tests.

Current state: Flag is ON locally ✓
```

### Step 3: Test ON State (New Feature)
**What the agent does:**
- Guide testing new feature code path
- Identify key scenarios to test
- Check for visual/functional regressions
- Test error handling
- Validate performance

**Context needed:**
- Feature requirements
- Acceptance criteria
- Known edge cases

**Example:**
```
Testing ON State (New Checkout):

Manual Testing Checklist:
✓ 1. Add item to cart
✓ 2. Proceed to checkout
✓ 3. New checkout UI appears
✓ 4. Fill payment info
✓ 5. Complete purchase
✓ 6. Verify order confirmation

Edge Cases to Test:
□ Empty cart → checkout
□ Invalid payment info
□ Session timeout during checkout
□ Network error during submission

Automated Test:
describe('New Checkout', () => {
  beforeEach(() => {
    Rox.overrides.setOverride('feature_new_checkout_page', true);
  });

  it('completes checkout successfully', async () => {
    // Test implementation
  });
});
```

### Step 4: Test OFF State (Fallback/Original)
**What the agent does:**
- Guide testing original code path
- Ensure nothing broke
- Verify graceful fallback
- Test default behavior intact
- Check for conditional logic errors

**Context needed:**
- Original feature behavior
- Fallback expectations
- Regression risk areas

**Example:**
```
Testing OFF State (Old Checkout):

CRITICAL: Many developers forget this step!

Manual Testing Checklist:
Set flag to OFF:
Rox.overrides.setOverride('feature_new_checkout_page', false);

✓ 1. Old checkout UI appears (not new one)
✓ 2. Complete purchase with old flow
✓ 3. Verify original functionality intact
✓ 4. Check for any errors in console

Common Issues to Check:
□ New code accidentally runs when flag is OFF
□ CSS/styles leak between versions
□ Shared state conflicts
□ Cleanup not happening

Automated Test:
it('uses old checkout when flag disabled', async () => {
  Rox.overrides.setOverride('feature_new_checkout_page', false);
  // Verify old checkout appears
});
```

### Step 5: Test State Transitions & Edge Cases
**What the agent does:**
- Test switching flag ON → OFF → ON
- Test during active user session
- Test with different user segments
- Identify race conditions
- Test cache invalidation

**Context needed:**
- State management approach
- Caching behavior
- User session handling

**Example:**
```
Advanced Testing Scenarios:

1. Flag Toggle During Session:
   - User starts checkout (flag ON)
   - Flag switched to OFF mid-session
   - Verify graceful handling (no crash)

2. Different User Contexts:
   - Test as beta user (should see new)
   - Test as regular user (should see old)
   - Verify targeting works correctly

3. Performance Testing:
   - Measure page load with flag ON
   - Measure page load with flag OFF
   - Compare metrics (no major regression)

4. Cache Busting:
   - Change flag value
   - Verify SDK picks up change
   - Check cache invalidation works

All scenarios passed? ✓
Ready for deployment!
```

## Required Skills

### Core Skills
- [ ] **flag-impact-analyzer** - Identifies what flag controls
  - Parse code to find flag usage
  - Identify affected user flows
  - Determine ON vs OFF behaviors
  - Flag critical vs non-critical paths

- [ ] **local-testing-configurator** - Sets up local test environment
  - Guide SDK override configuration
  - Set up test user contexts
  - Configure test data
  - Explain testing modes

- [ ] **test-scenario-generator** - Creates test cases
  - Generate ON state test cases
  - Generate OFF state test cases
  - Identify edge cases
  - Create test checklist

- [ ] **test-code-generator** - Writes automated tests
  - Generate test code for user's framework
  - Create flag override setup
  - Write assertion logic
  - Provide test examples

- [ ] **test-validator** - Verifies testing completeness
  - Check both states tested
  - Verify edge cases covered
  - Validate test coverage
  - Confirm readiness for deployment

### Supporting Skills
- [ ] **performance-test-helper** - Guides performance testing with flags
- [ ] **integration-test-helper** - Tests flag with external systems
- [ ] **visual-regression-tester** - Catches UI changes
- [ ] **test-data-generator** - Creates realistic test data

## Constraints & Guardrails

**Must NOT do:**
- Allow deployment without testing OFF state
- Skip edge case testing
- Test only happy path
- Forget to test flag transitions
- Ignore performance implications

**Must ALWAYS do:**
- Test both ON and OFF states
- Test state transitions
- Test with different user contexts
- Write automated tests
- Check for regressions
- Validate error handling
- Confirm production-readiness

## Edge Cases

1. **Case:** Flag controls complex feature with many code paths
   **Handling:** Break into smaller test scenarios, use test matrix

2. **Case:** Flag depends on other flags (flag dependencies)
   **Handling:** Test all combinations, create dependency test matrix

3. **Case:** Flag behavior differs by environment
   **Handling:** Test environment-specific configurations, document differences

4. **Case:** Flag affects third-party integrations
   **Handling:** Use mocks/stubs, test integration separately

5. **Case:** Flag changes database schema or data
   **Handling:** Test data migrations, rollback procedures, data integrity

6. **Case:** Flag has time-based behavior
   **Handling:** Mock time in tests, test scheduled rollouts

7. **Case:** Flag testing requires specific user permissions
   **Handling:** Create test users with required roles, test permission logic

## Success Metrics
- [ ] Both ON and OFF states tested: 100%
- [ ] Automated tests written: 90%+ of flagged features
- [ ] Zero flag-related production bugs in first 7 days
- [ ] Test coverage maintained or improved
- [ ] Developer confidence: "Thoroughly tested" (95%+ yes)
- [ ] Time spent testing: 15-20 minutes per flag

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Follows: JOURNEY-05-new-flag-setup-wizard
- Works with: JOURNEY-06-target-group-builder
- Precedes: JOURNEY-08-gradual-rollout-planner
- Quality gate before: All rollout journeys
