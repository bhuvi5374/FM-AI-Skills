# Journey: Target Group Builder

## Overview
**Goal:** Help users create effective target groups for precise feature flag targeting based on user attributes, behaviors, and segments.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Help me create a target group for beta users"
- "I want to target only premium subscribers"
- "Show this feature to users in the US only"
- "Create a segment for internal testing"

**Success Criteria:**
- Target group created with appropriate conditions
- Custom properties configured correctly
- Group size estimated
- Preview shows matching users
- User understands how targeting works
- Group ready to use in flag rollouts

## User Persona
- **Role:** Product Manager, Developer, QA Engineer
- **Pain Point:**
  - Unsure how to structure targeting rules
  - Complex boolean logic is confusing (AND/OR)
  - Don't know what custom properties are available
  - Can't preview who will match targeting
  - Fear of targeting wrong users in production
  - Need to test with specific user segments

- **Current Manual Process:**
  1. Guess at targeting syntax
  2. Trial and error with rules
  3. Deploy to production to see who matches
  4. Fix mistakes after users report issues

  Time: 30-60 minutes, potential production issues

## Agentic Happy Path

### Step 1: Define Targeting Intent
**What the agent does:**
- Ask who should see the feature
- Understand targeting criteria
- Identify if simple or complex targeting needed
- Check if custom properties exist

**Context needed:**
- Available custom properties in FM
- User's understanding of their user base
- Targeting complexity level

**Example:**
```
Agent: "Who should see this feature?"
User: "Only beta users with premium accounts"

Agent: "Let's break that down:
1. Beta users - Do you have a 'beta_user' property?
2. Premium accounts - Is this 'subscription_tier' = 'premium'?"

User: "Yes, we have both properties"

Agent: "Perfect! This needs two conditions (AND logic).
Let me help you build this."
```

### Step 2: Identify Available Properties
**What the agent does:**
- Show list of existing custom properties
- Explain property types (string, number, boolean, semver)
- Suggest which properties to use
- Guide creating new properties if needed

**Context needed:**
- Existing custom properties in FM
- User attribute schema
- Property value examples

**Example:**
```
Available Custom Properties:
✓ user_id (string)
✓ email (string)
✓ subscription_tier (string) - Values: free, premium, enterprise
✓ beta_user (boolean)
✓ account_created_date (date)
✓ country (string)
✓ feature_usage_count (number)

Recommended for your use case:
- beta_user = true
- subscription_tier = "premium"
```

### Step 3: Build Targeting Rules
**What the agent does:**
- Create conditions based on user's criteria
- Explain AND vs OR logic
- Show rule preview
- Validate syntax
- Test with example users

**Context needed:**
- Boolean logic understanding
- Property value formats
- Matching algorithms (exact, regex, etc.)

**Example:**
```
Target Group: "Beta Premium Users"

Rule Structure:
Condition 1: beta_user equals true
AND
Condition 2: subscription_tier equals "premium"

This matches users who:
- Have beta_user flag set to true
- AND subscription tier is "premium"

Example matching users:
✓ user123@example.com (beta: true, tier: premium)
✓ alice@test.com (beta: true, tier: premium)
✗ bob@test.com (beta: true, tier: free) - Wrong tier
✗ carol@test.com (beta: false, tier: premium) - Not beta

Does this look correct?
```

### Step 4: Estimate & Preview Audience
**What the agent does:**
- Estimate number of matching users
- Show sample of matching users (if available)
- Calculate percentage of total user base
- Warn if audience is too large or too small

**Context needed:**
- Total user count
- User property distributions
- Sampling capabilities

**Example:**
```
Audience Estimation:

Total users in system: 50,000
Matching this target group: ~2,500 users (5%)

Sample matching users:
- user_abc123 (beta: true, tier: premium)
- user_def456 (beta: true, tier: premium)
- user_ghi789 (beta: true, tier: premium)
[View 50 more samples]

This is a reasonable group size for gradual rollout.

Warning: If you expect more/fewer users, check your
conditions are correct.
```

### Step 5: Create & Validate Group
**What the agent does:**
- Create target group in CloudBees FM
- Assign descriptive name
- Add documentation
- Test with known user IDs
- Make group available for flag targeting

**Context needed:**
- FM API access
- Target group naming conventions
- Test user IDs

**Example:**
```
Target Group Created:

Name: beta_premium_users
Description: "Beta testers with premium subscriptions"
Rules:
  - beta_user = true AND
  - subscription_tier = "premium"

Estimated size: 2,500 users (5%)

Test with your user ID:
Enter user_id: user123@example.com
✓ This user MATCHES the target group

Target group is ready to use in flag rollouts!

Usage:
Go to any flag → Targeting → Select "beta_premium_users"
```

## Required Skills

### Core Skills
- [ ] **targeting-intent-parser** - Understands user's targeting needs
  - Parse natural language targeting criteria
  - Identify simple vs complex targeting
  - Break down compound requirements
  - Suggest targeting approach

- [ ] **custom-property-advisor** - Guides property selection
  - List available custom properties
  - Explain property types
  - Suggest relevant properties
  - Guide creating new properties if needed

- [ ] **targeting-rule-builder** - Creates targeting conditions
  - Build condition syntax
  - Explain AND/OR logic
  - Validate rule structure
  - Show rule preview

- [ ] **audience-estimator** - Estimates target group size
  - Calculate matching users
  - Provide percentage of user base
  - Show sample matching users
  - Warn about unusual sizes

- [ ] **target-group-creator** - Creates group via FM API
  - Call CloudBees FM API
  - Set group metadata
  - Validate group creation
  - Test with sample users

### Supporting Skills
- [ ] **boolean-logic-explainer** - Teaches AND/OR/NOT logic
- [ ] **regex-pattern-helper** - Assists with pattern matching
- [ ] **semver-targeting-helper** - Helps target by version ranges
- [ ] **geo-targeting-helper** - Assists with location-based targeting
- [ ] **percentage-rollout-helper** - Combines target groups with percentages

## Constraints & Guardrails

**Must NOT do:**
- Create overly broad target groups without warning (>50% of users)
- Use personally identifiable information without privacy notice
- Create contradictory conditions (impossible to match)
- Skip audience estimation
- Allow targeting without testing

**Must ALWAYS do:**
- Validate conditions make logical sense
- Estimate audience size before creation
- Provide test functionality with sample users
- Warn if group matches 0 users
- Document group purpose clearly
- Explain AND vs OR logic clearly
- Show matching examples

## Edge Cases

1. **Case:** Targeting rules result in 0 matching users
   **Handling:** Alert user, help debug conditions, suggest corrections

2. **Case:** Target group matches nearly all users (>90%)
   **Handling:** Confirm intentional, suggest inverse logic for efficiency

3. **Case:** Complex nested conditions (multiple AND/OR)
   **Handling:** Visualize logic tree, simplify if possible, test thoroughly

4. **Case:** Custom property doesn't exist yet
   **Handling:** Guide property creation, set up SDK to send property

5. **Case:** User wants to exclude specific users (NOT logic)
   **Handling:** Explain exclusion syntax, show examples

6. **Case:** Dynamic properties (user behavior over time)
   **Handling:** Explain property update requirements, freshness considerations

7. **Case:** Targeting by multiple countries/regions
   **Handling:** Use list matching, explain OR logic within condition

## Success Metrics
- [ ] Target group created correctly in under 5 minutes
- [ ] Audience estimation within 10% of actual
- [ ] Zero targeting logic errors
- [ ] User understands boolean logic (post-survey)
- [ ] 95%+ groups have non-zero matches
- [ ] User satisfaction: 4+ stars

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Works with: JOURNEY-05-new-flag-setup-wizard
- Used by: JOURNEY-08-gradual-rollout-planner, JOURNEY-09-rollout-impact-analyzer
- Enables: Precise feature targeting
