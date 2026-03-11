# Journey: Flag Dependency Mapper

## Overview
**Goal:** Visualize and understand flag dependencies to safely manage complex flag relationships and determine safe removal order.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "What other flags depend on this one?"
- "Show me the flag dependency graph"
- "Which flags should I clean up first?"
- "Map dependencies between my flags"

**Success Criteria:**
- Flag dependencies identified
- Dependency graph visualized
- Safe removal order determined
- Circular dependencies flagged
- User understands relationships
- Safe cleanup plan provided

## User Persona
- **Role:** Tech Lead, Engineering Manager, Senior Developer
- **Pain Point:**
  - Flags have complex relationships
  - Unsure what breaks if flag is removed
  - Circular dependencies cause issues
  - Need to understand flag ecosystem
  - Want to clean up safely
  - Lack visibility into dependencies

- **Current Manual Process:**
  1. Guess which flags depend on others
  2. Search code manually for relationships
  3. Trial and error cleanup
  4. Break things and fix later

  Time: Hours of investigation, risky

## Agentic Happy Path

### Step 1: Select Analysis Scope
**What the agent does:**
- Ask which flags to analyze
- Determine scope (single flag, all flags, subset)
- Identify analysis depth needed
- Set context boundaries

**Context needed:**
- Available flags
- User's goal (cleanup, understanding, debugging)
- System complexity

**Example:**
```
Agent: "What do you want to analyze?"

Options:
1. Single flag dependencies
2. All flags in application
3. Subset by tag/area
4. Flags marked for cleanup

User: "All flags in checkout application"

Agent: "Analyzing 23 flags in checkout app...
This will show both:
- Configured dependencies (FM settings)
- Code-level dependencies (implicit)

Starting dependency scan..."
```

### Step 2: Discover Configured Dependencies
**What the agent does:**
- Query FM for explicit flag dependencies
- Check flag configuration settings
- Identify parent/child relationships
- Find prerequisite flags

**Context needed:**
- CloudBees FM API access
- Flag configuration data
- Dependency settings

**Example:**
```
Configured Dependencies:

Flag: feature_new_checkout_v3
Depends on:
→ feature_new_payment_processor (required)
→ feature_address_autocomplete (optional)

Dependents (flags that depend on this):
← experiment_one_click_checkout (requires this ON)
← rollout_express_checkout (requires this ON)

Flag: feature_new_payment_processor
Depends on:
→ None (independent)

Dependents:
← feature_new_checkout_v3 (required)
← feature_subscription_billing (required)

These are explicit dependencies configured
in CloudBees FM settings.
```

### Step 3: Analyze Code-Level Dependencies
**What the agent does:**
- Scan code for implicit dependencies
- Identify nested flag checks
- Find conditional relationships
- Detect runtime dependencies

**Context needed:**
- Code repository access
- Programming language
- SDK patterns

**Example:**
```
Code-Level Dependencies (Implicit):

File: src/checkout/CheckoutPage.tsx

FOUND:
if (isEnabled('feature_new_checkout_v3')) {
  // Nested check inside!
  if (isEnabled('feature_shipping_calculator')) {
    // Use new shipping calculator
  }
}

DEPENDENCY DETECTED:
feature_shipping_calculator is only used when
feature_new_checkout_v3 is ON.

Implicit dependency:
feature_new_checkout_v3 → feature_shipping_calculator

File: src/payment/PaymentProcessor.ts

FOUND:
const processor = isEnabled('feature_new_payment_processor')
  ? new StripeProcessor()
  : new LegacyProcessor();

if (processor instanceof StripeProcessor) {
  // This only runs when new payment processor is ON
  const fastCheckout = isEnabled('feature_express_checkout');
}

DEPENDENCY DETECTED:
feature_express_checkout logic is inside
feature_new_payment_processor block.

Runtime dependency:
feature_new_payment_processor → feature_express_checkout
```

### Step 4: Visualize Dependency Graph
**What the agent does:**
- Create visual dependency graph
- Show parent-child relationships
- Highlight circular dependencies
- Color-code by status
- Generate multiple views

**Context needed:**
- Graph generation capability
- Visualization preferences
- Output format

**Example:**
```
Dependency Graph: Checkout Application

┌─────────────────────────────────────────────────┐
│                                                 │
│  feature_new_payment_processor                  │
│  (100% - Root dependency)                       │
│                                                 │
└─────────────────┬──────────────┬────────────────┘
                  │              │
         ┌────────▼─────┐  ┌─────▼──────────┐
         │ feature_     │  │ feature_       │
         │ new_checkout │  │ subscription_  │
         │ _v3          │  │ billing        │
         │ (75%)        │  │ (100%)         │
         └──────┬───────┘  └────────────────┘
                │
       ┌────────┼────────┐
       │        │        │
  ┌────▼─┐ ┌───▼──┐ ┌───▼───────┐
  │exp_  │ │roll_ │ │feature_   │
  │one_  │ │out_  │ │shipping_  │
  │click │ │exp   │ │calculator │
  │(10%) │ │(50%) │ │(100%)     │
  └──────┘ └──────┘ └───────────┘

Legend:
→ Configured dependency (FM)
⇢ Code-level dependency (implicit)
🔴 Circular dependency (warning)
🟢 Independent (no dependencies)

Safe Cleanup Order:
5. exp_one_click_checkout
4. rollout_express_checkout
3. feature_shipping_calculator
2. feature_new_checkout_v3
1. feature_new_payment_processor (last!)

NEVER delete feature_new_payment_processor first!
It would break 4 dependent flags.
```

### Step 5: Provide Cleanup Strategy
**What the agent does:**
- Determine safe removal order
- Flag circular dependencies
- Identify independent flags (safe to remove)
- Provide step-by-step plan
- Warn about risks

**Context needed:**
- Dependency analysis results
- Business priorities
- Risk tolerance

**Example:**
```
Dependency Analysis & Cleanup Strategy:

INDEPENDENT FLAGS (Safe to remove anytime):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. feature_promo_banner (no dependencies)
2. experiment_button_color (no dependencies)
3. rollout_new_footer (no dependencies)

→ These can be cleaned up in any order ✓

LEAF FLAGS (Remove these first):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. experiment_one_click_checkout
   - Depends on: feature_new_checkout_v3
   - Dependents: None
   → Clean up first ✓

2. feature_shipping_calculator
   - Depends on: feature_new_checkout_v3
   - Dependents: None
   → Clean up second ✓

MIDDLE TIER FLAGS (Clean after leafs):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
3. feature_new_checkout_v3
   - Depends on: feature_new_payment_processor
   - Dependents: 3 flags (clean them first!)
   → Clean up after experiment_one_click_checkout,
     rollout_express_checkout, feature_shipping_calculator

ROOT FLAGS (Clean last):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
4. feature_new_payment_processor
   - Depends on: None
   - Dependents: 4 flags
   → ONLY clean after ALL dependents removed ⚠️

CIRCULAR DEPENDENCY DETECTED: 🔴
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
feature_A depends on feature_B
feature_B depends on feature_A

This is a CODE ERROR!
Resolution:
1. One flag should not depend on the other
2. Refactor code to remove circular dependency
3. Then clean up normally

Recommended Cleanup Timeline:
- Week 1: Clean up 3 independent flags
- Week 2: Clean up 2 leaf flags
- Week 3: Clean up middle tier
- Week 4: Clean up root (if all dependents gone)

Total cleanup time: 1 month (conservative)
```

## Required Skills

### Core Skills
- [ ] **configured-dependency-scanner** - Finds FM dependencies
  - Query CloudBees FM API
  - Get explicit dependencies
  - Identify parent/child relationships
  - Map configured prerequisites

- [ ] **code-dependency-analyzer** - Finds implicit dependencies
  - Scan code for nested flag checks
  - Identify conditional relationships
  - Detect runtime dependencies
  - Parse complex logic

- [ ] **dependency-graph-builder** - Creates visual graph
  - Build dependency tree
  - Identify graph structure
  - Detect circular dependencies
  - Generate visual representation

- [ ] **cleanup-order-calculator** - Determines safe order
  - Topological sort of dependencies
  - Identify independent flags
  - Find leaf nodes (clean first)
  - Calculate cleanup timeline

- [ ] **circular-dependency-detector** - Finds problematic cycles
  - Detect circular dependencies
  - Explain why they're problematic
  - Suggest resolution strategies

### Supporting Skills
- [ ] **graph-visualizer** - Creates diagrams and charts
- [ ] **dependency-validator** - Checks for broken dependencies
- [ ] **impact-simulator** - Predicts impact of flag removal
- [ ] **batch-cleanup-planner** - Plans bulk cleanup operations

## Constraints & Guardrails

**Must NOT do:**
- Recommend removing parent before children
- Miss implicit code-level dependencies
- Ignore circular dependencies
- Auto-remove without dependency check
- Skip impact analysis

**Must ALWAYS do:**
- Check both configured and code dependencies
- Identify circular dependencies
- Provide safe cleanup order
- Warn about dependent flags
- Visualize relationships clearly
- Validate dependency accuracy
- Consider business priority

## Edge Cases

1. **Case:** Circular dependency detected
   **Handling:** Flag error, explain issue, suggest refactoring, block cleanup

2. **Case:** Dynamic flag names (computed at runtime)
   **Handling:** Flag as "unable to verify", recommend manual review

3. **Case:** Flag dependencies across multiple repositories
   **Handling:** Scan all repos, show cross-repo dependencies

4. **Case:** Optional vs required dependencies
   **Handling:** Differentiate visually, explain impact of each

5. **Case:** Deep dependency chains (5+ levels)
   **Handling:** Provide layered view, show each level separately

6. **Case:** Flag depends on external system state
   **Handling:** Note external dependencies, cannot map fully

7. **Case:** Conflicting dependencies (A requires B ON, C requires B OFF)
   **Handling:** Flag configuration error, require resolution

## Success Metrics
- [ ] Dependency detection accuracy: 95%+
- [ ] Circular dependencies found: 100%
- [ ] Safe cleanup order correct: 100%
- [ ] Graph generation time: <30 seconds
- [ ] User understanding: "Clear relationships" (4.5+ stars)
- [ ] Zero broken dependencies from cleanup: 100%
- [ ] Cleanup time reduced: 50%+

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Enables: JOURNEY-15-flag-cleanup-assistant (safe cleanup order)
- Works with: JOURNEY-11-flag-health-audit
- Prevents: Breaking changes from dependency issues
- Run: Before major cleanup efforts
