# Journey: Flag Configuration Validator (Troubleshooting)

## Overview
**Goal:** Diagnose and fix flag configuration issues when flags don't work as expected across environments.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Why isn't my flag working in production?"
- "This flag should be ON but users see OFF"
- "Help me debug this flag configuration"
- "Flag works in dev but not in production"

**Success Criteria:**
- Issue identified and diagnosed
- Root cause determined
- Fix provided or guidance given
- Flag working as expected
- User understands what went wrong
- Prevention tips provided

## User Persona
- **Role:** Developer, QA Engineer, Support Engineer
- **Pain Point:**
  - Flag doesn't behave as expected
  - Different behavior across environments
  - Users report feature not appearing
  - Unclear where configuration is wrong
  - Debugging is time-consuming
  - Need fast resolution

- **Current Manual Process:**
  1. Check FM dashboard manually
  2. Compare environment configs
  3. Check SDK logs
  4. Ask team for help
  5. Trial and error fixes

  Time: 30-120 minutes per issue

## Agentic Happy Path

### Step 1: Understand The Problem
**What the agent does:**
- Ask what's expected vs actual behavior
- Identify which flag is problematic
- Determine which environment(s) affected
- Understand user context (who, when, where)
- Check if issue is consistent or intermittent

**Context needed:**
- Flag name
- Environment details
- Expected behavior
- Observed behavior

**Example:**
```
Agent: "What flag is having issues?"
User: "feature_new_checkout_page in production"

Agent: "What should happen vs what's actually happening?"
User: "Should show new checkout at 50% but users see old"

Agent: "Are ALL users seeing old, or some users?"
User: "All users report seeing old version"

Agent: "Let me diagnose this systematically..."
```

### Step 2: Check Flag Configuration
**What the agent does:**
- Query flag settings from CloudBees FM
- Verify rollout percentage per environment
- Check targeting rules
- Review flag type and defaults
- Compare across environments

**Context needed:**
- CloudBees FM API access
- Flag metadata
- Environment configurations

**Example:**
```
Configuration Check:

Flag: feature_new_checkout_page
Type: Boolean

Environment Settings:
┌──────────────┬──────────┬───────────────┬─────────┐
│ Environment  │ Rollout  │ Target Groups │ Default │
├──────────────┼──────────┼───────────────┼─────────┤
│ Local        │ 100%     │ All users     │ ON      │
│ Development  │ 100%     │ All users     │ ON      │
│ Staging      │ 50%      │ All users     │ OFF     │
│ Production   │ 50%      │ premium_users │ OFF     │
└──────────────┴──────────┴───────────────┴─────────┘

⚠️  ISSUE FOUND: Production has targeting rule!
Rollout: 50% of "premium_users" only
Regular users: NOT included in target group

This explains why users see old version!
```

### Step 3: Validate SDK Integration
**What the agent does:**
- Check if SDK is initialized correctly
- Verify SDK key matches environment
- Check SDK version compatibility
- Review SDK fetch frequency
- Check flag name spelling

**Context needed:**
- Application code
- SDK version
- SDK configuration
- Network logs

**Example:**
```
SDK Validation:

Application: checkout-service
SDK: @cloudbees/fm-node-sdk v4.2.1
Environment: Production

SDK Initialization:
✓ SDK key: prod_x7y8z9... (matches production)
✓ SDK version: 4.2.1 (latest)
✓ Connection: Active (last fetch 30s ago)

Flag Evaluation:
✓ Flag name: "feature_new_checkout_page" (correct)
✓ Fetch frequency: 30s (appropriate)
✓ Cache: Enabled

Potential Issues:
⚠️  User context not being passed!
   Code: Flags.feature_new_checkout_page.isEnabled()
   Missing: User ID/properties for targeting

When target groups are used, SDK needs user
context to determine if user matches group.

Current code doesn't pass user info!
```

### Step 4: Check User Context & Targeting
**What the agent does:**
- Verify user properties being sent
- Check if users match target groups
- Validate custom properties
- Test with specific user IDs
- Check property value formats

**Context needed:**
- Target group definitions
- User property schema
- Sample user IDs

**Example:**
```
User Context Check:

Target Group: premium_users
Rule: subscription_tier = "premium"

Testing with sample users:

User: user_123
Properties sent: { user_id: "user_123" }
⚠️  Missing: subscription_tier property!
Result: Does NOT match target group
Sees: Old checkout (OFF)

User: user_456
Properties sent: {
  user_id: "user_456",
  subscription_tier: "free"
}
Result: Does NOT match (tier is "free" not "premium")
Sees: Old checkout (OFF)

ROOT CAUSE IDENTIFIED:
Application is not sending "subscription_tier"
property to SDK. SDK cannot match users to
"premium_users" target group without this property.

FIX REQUIRED:
Update SDK initialization to include user properties:
Rox.setCustomUserProperties({
  subscription_tier: user.subscriptionTier
});
```

### Step 5: Provide Fix & Prevention
**What the agent does:**
- Explain root cause clearly
- Provide specific code fix
- Test fix with sample users
- Provide prevention tips
- Document for team learning

**Context needed:**
- Programming language
- Codebase structure
- Team knowledge base

**Example:**
```
ISSUE DIAGNOSIS:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Problem: Flag not working for any users in production
Root Cause: Missing user context (subscription_tier property)

Why It Happened:
- Production flag uses target group "premium_users"
- Target group requires subscription_tier property
- SDK not receiving this property from application
- Without property, NO users match target group
- Result: Everyone sees OFF (old checkout)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FIX:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Update: src/services/feature-flags.js

BEFORE:
Rox.setup(process.env.CLOUDBEES_SDK_KEY);

AFTER:
Rox.setCustomUserProperties({
  subscription_tier: user.subscriptionTier,
  user_id: user.id
});
Rox.setup(process.env.CLOUDBEES_SDK_KEY);

Test with user:
user_789 (premium subscriber)
Expected: See new checkout ✓
Verify: subscription_tier = "premium" sent

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PREVENTION TIPS:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Always test flags with target groups in staging first
2. Use SDK verbose logging during development
3. Verify user properties in SDK dashboard
4. Document required properties per flag
5. Add property validation in CI/CD

Estimated fix time: 15 minutes
Test in staging before production deployment!
```

## Required Skills

### Core Skills
- [ ] **issue-diagnosis-initiator** - Starts troubleshooting process
  - Gather problem symptoms
  - Identify affected flags and environments
  - Determine scope (consistent vs intermittent)
  - Set diagnostic approach

- [ ] **config-validator** - Checks flag configuration
  - Query flag settings via API
  - Verify rollout percentages
  - Check targeting rules
  - Compare environment configs
  - Flag misconfigurations

- [ ] **sdk-integration-checker** - Validates SDK setup
  - Check SDK initialization
  - Verify SDK key
  - Check SDK version
  - Review connection status
  - Validate flag evaluation

- [ ] **user-context-validator** - Verifies user targeting
  - Check user properties being sent
  - Validate against target groups
  - Test with sample users
  - Flag missing properties

- [ ] **fix-generator** - Provides solutions
  - Explain root cause
  - Generate code fixes
  - Provide testing guidance
  - Document prevention tips

### Supporting Skills
- [ ] **network-inspector** - Checks API connectivity
- [ ] **cache-analyzer** - Investigates caching issues
- [ ] **log-analyzer** - Parses SDK and application logs
- [ ] **permission-checker** - Verifies access permissions

## Constraints & Guardrails

**Must NOT do:**
- Make changes directly to production without approval
- Recommend disabling flags without understanding impact
- Skip root cause analysis
- Provide generic "try this" without diagnosis
- Ignore environment differences

**Must ALWAYS do:**
- Systematically diagnose before suggesting fixes
- Check all configuration layers (flag, env, SDK, user)
- Provide specific, actionable fixes
- Test fixes in non-production first
- Explain WHY issue occurred
- Provide prevention guidance
- Document for team learning

## Edge Cases

1. **Case:** Issue is network connectivity (firewall, proxy)
   **Handling:** Check SDK connection logs, provide network troubleshooting

2. **Case:** SDK version incompatibility
   **Handling:** Identify version mismatch, provide upgrade path

3. **Case:** Caching causing stale flag values
   **Handling:** Check cache TTL, provide cache invalidation

4. **Case:** Issue only for specific users/regions
   **Handling:** Analyze user properties, geo-targeting, network paths

5. **Case:** Timing issue (flag works sometimes)
   **Handling:** Check fetch frequency, analyze timing patterns

6. **Case:** Multiple flags interfering with each other
   **Handling:** Check flag dependencies, analyze interaction

7. **Case:** Configuration recently changed
   **Handling:** Review audit log, compare before/after states

## Success Metrics
- [ ] Issue diagnosis time: <10 minutes
- [ ] Fix accuracy: 95%+
- [ ] Resolution time: <30 minutes
- [ ] Root cause identified: 100%
- [ ] Prevention tips provided: 100%
- [ ] User satisfaction: "Issue resolved" (4.5+ stars)
- [ ] Recurring issues: <5%

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Emergency support for: All flag-related journeys
- Works with: JOURNEY-07-flag-testing-assistant (prevention)
- Complements: JOURNEY-12-flag-performance-analyzer
