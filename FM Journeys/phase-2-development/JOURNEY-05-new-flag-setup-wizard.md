# Journey: New Flag Setup Wizard

## Overview
**Goal:** Streamline creation of new feature flags with best practices, proper naming, environment configuration, Jira linking, and code snippets for experienced users.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Create a new flag for dark mode feature"
- "I need a flag for the checkout redesign"
- "Set up a flag for A/B testing the new pricing page"
- "Help me create a flag quickly"

**Success Criteria:**
- Flag created with proper naming convention
- Linked to relevant Jira ticket
- Environment defaults configured appropriately
- Code snippet generated and ready to use
- Initial tests created
- Team notified (if Slack integrated)

## User Persona
- **Role:** Developer, Product Manager
- **Pain Point:**
  - Creating flags manually is repetitive
  - Easy to forget best practices
  - Inconsistent naming across team
  - Forgetting to link Jira tickets
  - Need to look up SDK syntax each time
  - Want to speed up flag creation workflow

- **Current Manual Process:**
  1. Go to FM UI and create flag
  2. Think of appropriate name
  3. Set defaults for each environment
  4. Remember to link Jira ticket
  5. Look up SDK documentation for syntax
  6. Copy-paste code into IDE

  Time: 10-15 minutes per flag

## Agentic Happy Path

### Step 1: Understand Feature Context
**What the agent does:**
- Ask about the feature being flagged
- Identify feature scope (frontend/backend/full-stack)
- Determine flag type needed (boolean/string/number)
- Check if related Jira ticket exists
- Understand rollout expectations

**Context needed:**
- User's description of feature
- Existing flags in system
- Jira integration status
- Team naming conventions

**Example:**
```
Agent: "What feature are you creating a flag for?"
User: "We're redesigning the checkout page"

Agent: "Is this a full replacement or gradual rollout?"
User: "Gradual rollout, we want to A/B test it"

Agent: "Got it! I'll create a boolean flag for this.
Do you have a Jira ticket?"
User: "Yes, SHOP-456"

Agent: "Perfect, I'll link it."
```

### Step 2: Generate Flag Name
**What the agent does:**
- Suggest flag name following team conventions
- Validate name doesn't conflict with existing flags
- Apply naming patterns (feature_, experiment_, etc.)
- Get user approval for name

**Context needed:**
- Team naming conventions
- Existing flag names
- Feature domain/area
- Flag purpose (rollout vs experiment vs ops)

**Example:**
```
Suggested Flag Name:
feature_new_checkout_page

Based on:
- Prefix: "feature_" (new functionality)
- Domain: "checkout"
- Descriptor: "new_checkout_page"

Alternative names:
- experiment_checkout_redesign
- rollout_checkout_v2

Does this name work for you?
User: "Yes, use feature_new_checkout_page"
```

### Step 3: Configure Flag Settings
**What the agent does:**
- Set appropriate default values per environment
- Configure flag description
- Add relevant tags
- Link to Jira ticket
- Set rollout strategy hints
- Apply team-specific templates

**Context needed:**
- Environment configuration
- Rollout best practices
- Team tagging conventions
- Jira integration

**Example:**
```
Flag Configuration:

Name: feature_new_checkout_page
Type: Boolean
Description: "Enables redesigned checkout page (SHOP-456)"
Tags: checkout, frontend, experiment, q1-2026
Jira: SHOP-456 (linked ✓)

Default Values:
- Local: ON (for easy testing)
- Development: ON (team testing)
- Staging: OFF → Gradual (A/B test here first)
- Production: OFF (manual rollout)

Rollout Strategy: Gradual (0% → 10% → 50% → 100%)
Approval Required: Yes (Production only)
```

### Step 4: Generate Code Snippets
**What the agent does:**
- Detect user's programming language from codebase
- Generate SDK-specific code snippet
- Show implementation example with context
- Provide test case template
- Explain flag usage patterns

**Context needed:**
- Programming language/framework
- SDK version
- Code style preferences
- Testing framework

**Example:**
```
Code Integration (React):

1. Check the flag:
   import { useFlag } from '@cloudbees/fm-react-sdk';

   function CheckoutPage() {
     const isNewCheckout = useFlag('feature_new_checkout_page');

     return isNewCheckout ? (
       <NewCheckoutPage />
     ) : (
       <OldCheckoutPage />
     );
   }

2. Test case (Jest):
   describe('CheckoutPage', () => {
     it('shows new checkout when flag enabled', () => {
       mockFlag('feature_new_checkout_page', true);
       render(<CheckoutPage />);
       expect(screen.getByText('New Checkout')).toBeInTheDocument();
     });
   });

Code snippet copied to clipboard ✓
```

### Step 5: Finalize & Notify
**What the agent does:**
- Create the flag in CloudBees FM
- Verify flag is accessible via SDK
- Send Slack notification (if configured)
- Add to team flag tracking
- Provide next steps

**Context needed:**
- FM API access
- Slack integration
- Team notification preferences

**Example:**
```
Flag Created Successfully!

✓ feature_new_checkout_page is live
✓ Linked to SHOP-456
✓ Code snippet ready to use
✓ Team notified in #feature-flags

Next steps:
1. Add the code snippet to your branch
2. Test locally (flag is ON in Local env)
3. Create PR with your feature
4. We'll handle rollout after PR merges

Flag details: [View in FM Dashboard]
```

## Required Skills

### Core Skills
- [ ] **feature-context-analyzer** - Understands feature being flagged
  - Parse user's feature description
  - Identify feature scope
  - Determine appropriate flag type
  - Detect related work (Jira tickets)

- [ ] **flag-name-generator** - Creates standards-compliant flag names
  - Apply team naming conventions
  - Check for conflicts with existing flags
  - Suggest multiple name options
  - Validate name quality

- [ ] **flag-configurator** - Sets up flag with best practices
  - Configure environment defaults
  - Set appropriate metadata (description, tags)
  - Link to Jira/external systems
  - Apply rollout strategy templates

- [ ] **code-snippet-generator** - Creates language-specific code
  - Detect programming language
  - Generate SDK-appropriate code
  - Provide usage examples
  - Create test templates

- [ ] **flag-creator** - Creates flag via FM API
  - Call CloudBees FM API
  - Validate flag creation
  - Verify SDK accessibility
  - Handle creation errors

- [ ] **team-notifier** - Sends notifications
  - Post to Slack/Teams
  - Update team tracking
  - Generate announcement

### Supporting Skills
- [ ] **jira-ticket-linker** - Links flags to Jira issues
- [ ] **flag-template-applier** - Uses team-specific templates
- [ ] **flag-naming-validator** - Enforces naming standards
- [ ] **duplicate-flag-detector** - Prevents duplicate flags for same feature

## Constraints & Guardrails

**Must NOT do:**
- Create flags without checking for duplicates
- Use non-standard naming without user confirmation
- Skip Jira linking when ticket is available
- Create production-default-ON flags without explicit approval
- Generate code in wrong programming language

**Must ALWAYS do:**
- Validate flag name follows conventions
- Check for existing flags with similar purpose
- Link to Jira ticket if provided
- Configure safe environment defaults (prod OFF)
- Generate appropriate code snippets
- Verify flag is created successfully
- Provide clear next steps

## Edge Cases

1. **Case:** Similar flag already exists
   **Handling:** Alert user, show existing flag, ask if new one still needed

2. **Case:** User wants non-standard naming
   **Handling:** Allow with warning, document reason in flag description

3. **Case:** Jira ticket not found
   **Handling:** Offer to create flag anyway, remind to link later

4. **Case:** User's language/SDK not in database
   **Handling:** Provide generic example, offer to learn their stack

5. **Case:** Flag for sensitive feature (security, payments)
   **Handling:** Add extra approval requirements, special tags, notify security team

6. **Case:** User wants same flag across multiple apps
   **Handling:** Explain flag scope, create per-app or suggest shared flag

7. **Case:** Rush flag creation (skip best practices)
   **Handling:** Allow fast path but warn about risks, offer to enhance later

## Success Metrics
- [ ] Flag creation time: Under 3 minutes
- [ ] 100% flags follow naming conventions
- [ ] 90%+ flags linked to Jira tickets
- [ ] Zero duplicate flags created
- [ ] User satisfaction: 4.5+ stars
- [ ] Code snippet accuracy: 95%+

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Follows: JOURNEY-04-first-flag-creation-tutorial
- Works with: JOURNEY-06-target-group-builder, JOURNEY-08-gradual-rollout-planner
- Prerequisite for: All rollout and testing journeys
