# Journey: Flag Migration Helper

## Overview
**Goal:** Assist in migrating feature flags from another platform (LaunchDarkly, Unleash, Split, etc.) to CloudBees Feature Management with minimal disruption.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "We're migrating from LaunchDarkly to CloudBees FM"
- "Help me migrate my flags to CloudBees"
- "Import flags from our old system"
- "I need to switch feature flag platforms"

**Success Criteria:**
- All flags identified and inventoried
- Flags migrated to CloudBees FM
- Code references updated
- Feature parity validated
- Rollout states preserved
- Zero downtime during migration
- Team trained on new platform

## User Persona
- **Role:** Engineering Manager, Platform Engineer, Senior Developer
- **Pain Point:**
  - Switching FM platforms is risky
  - Hundreds of flags to migrate
  - Code must be updated
  - Can't have downtime
  - Need to preserve rollout states
  - Team needs training
  - Migration is complex and time-consuming

- **Current Manual Process:**
  1. Export flags from old system manually
  2. Create flags in CloudBees FM one-by-one
  3. Update all code references
  4. Hope nothing breaks
  5. Long migration period

  Time: Weeks to months, high risk

## Agentic Happy Path

### Step 1: Assess Current Platform
**What the agent does:**
- Identify source platform (LaunchDarkly, Unleash, etc.)
- Count total flags to migrate
- Understand current usage patterns
- Assess migration complexity
- Create migration timeline

**Context needed:**
- Source platform name
- Access to source platform
- Number of flags
- Code repositories

**Example:**
```
Agent: "Which platform are you migrating from?"
User: "LaunchDarkly"

Agent: "Do you have API access?"
User: "Yes, I have an admin API key"

Agent: "Connecting to LaunchDarkly...

Migration Assessment:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Source: LaunchDarkly
Total flags: 127
- Boolean flags: 98
- String flags: 21
- Number flags: 8

Environments:
- Production, Staging, Development

Code repositories: 3
- Frontend (React)
- Backend API (Node.js)
- Mobile App (React Native)

Migration Complexity: MEDIUM
Estimated time: 2-3 weeks
Approach: Phased migration (minimize risk)
```

### Step 2: Export & Inventory Flags
**What the agent does:**
- Export all flags from source platform
- Map flag types to CloudBees equivalents
- Capture current rollout states
- Document targeting rules
- Preserve flag metadata

**Context needed:**
- Source platform API access
- Export format
- Mapping rules
- Data transformation logic

**Example:**
```
Exporting from LaunchDarkly...

Flag Export Complete:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Sample Flag: new-checkout-flow

LaunchDarkly Config:
{
  "name": "new-checkout-flow",
  "key": "new-checkout-flow",
  "kind": "boolean",
  "environments": {
    "production": {
      "on": true,
      "fallthrough": { "variation": 0 },
      "offVariation": 1,
      "targets": [
        {
          "values": ["user-123", "user-456"],
          "variation": 0
        }
      ],
      "rules": [
        {
          "clauses": [
            {
              "attribute": "email",
              "op": "endsWith",
              "values": ["@company.com"]
            }
          ],
          "variation": 0
        }
      ]
    }
  }
}

CloudBees FM Mapping:
{
  "name": "new_checkout_flow",
  "type": "boolean",
  "environments": {
    "production": {
      "enabled": true,
      "defaultValue": false,
      "targetGroups": [
        {
          "name": "internal_users",
          "conditions": [
            {
              "property": "email",
              "operator": "endsWith",
              "value": "@company.com"
            }
          ]
        }
      ]
    }
  }
}

Compatibility: ✓ Full mapping possible
Notes: Targeting rules translated successfully
```

### Step 3: Create Migration Plan
**What the agent does:**
- Determine migration strategy (big bang vs phased)
- Create migration phases
- Identify flag migration order
- Plan code update strategy
- Define rollback procedure

**Context needed:**
- Business constraints
- Risk tolerance
- Team capacity
- Deployment frequency

**Example:**
```
Migration Strategy: PHASED (Recommended)

Why phased?
- Minimize risk (validate each phase)
- Team learns gradually
- Easy rollback per phase
- Business continuity maintained

PHASE 1: Preparation (Week 1)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ Set up CloudBees FM environments
□ Import all flags (disabled initially)
□ Validate flag configurations
□ No code changes yet

PHASE 2: Dual-Run Setup (Week 2)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ Add CloudBees SDK alongside LaunchDarkly
□ Evaluate both SDKs, use LD results
□ Monitor for discrepancies
□ Validate parity

PHASE 3: Test Environment Migration (Week 3)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ Switch dev environment to CloudBees
□ Run full test suite
□ Team tests new UI/workflows
□ Fix any issues

PHASE 4: Production Migration (Week 4)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ Switch 10% of prod traffic to CloudBees
□ Monitor for 48 hours
□ Increase to 50%, then 100%
□ Remove LaunchDarkly SDK
□ Decommission LaunchDarkly account

Rollback Plan: Keep LaunchDarkly active
until 100% confirmed on CloudBees.
```

### Step 4: Execute Migration (Code Updates)
**What the agent does:**
- Generate code migration scripts
- Update SDK imports
- Replace flag checks
- Update configuration
- Create migration PRs

**Context needed:**
- Programming languages
- Codebase structure
- SDK documentation
- Code modification permissions

**Example:**
```
Code Migration: Frontend (React)

BEFORE (LaunchDarkly):
```javascript
import { useLDClient } from 'launchdarkly-react-client-sdk';

function CheckoutPage() {
  const ldClient = useLDClient();
  const newCheckout = ldClient.variation('new-checkout-flow', false);

  return newCheckout ? <NewCheckout /> : <OldCheckout />;
}
```

AFTER (CloudBees FM):
```javascript
import { useFlag } from '@cloudbees/fm-react-sdk';

function CheckoutPage() {
  const newCheckout = useFlag('new_checkout_flow');

  return newCheckout ? <NewCheckout /> : <OldCheckout />;
}
```

Changes:
1. Replace import
2. Replace hook usage
3. Update flag name format (dash → underscore)
4. Simplified API (CloudBees cleaner)

Migration Script Created:
- frontend-migration.sh
- Runs codemod to auto-update
- Covers 234 flag references
- Estimated time: 10 minutes

Run script? [Yes/No]
```

### Step 5: Validate & Complete Migration
**What the agent does:**
- Validate feature parity
- Test in all environments
- Compare metrics before/after
- Verify no regressions
- Decommission old platform
- Update team documentation

**Context needed:**
- Test results
- Monitoring data
- Success criteria
- Team sign-off

**Example:**
```
Migration Validation:

PHASE 4: Production Complete ✓
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Environment Status:
✓ Development: 100% CloudBees FM
✓ Staging: 100% CloudBees FM
✓ Production: 100% CloudBees FM
✓ LaunchDarkly SDK removed from all apps

Feature Parity Check:
┌─────────────────┬──────────────┬──────────────┬────────┐
│ Flag            │ LD State     │ CB State     │ Match? │
├─────────────────┼──────────────┼──────────────┼────────┤
│ new-checkout    │ 75% ON       │ 75% ON       │ ✓      │
│ dark-mode       │ 100% ON      │ 100% ON      │ ✓      │
│ beta-features   │ 10% (users)  │ 10% (users)  │ ✓      │
│ ... (127 more)  │              │              │ ✓      │
└─────────────────┴──────────────┴──────────────┴────────┘

All flags: MATCH ✓

Metrics Comparison (Week before vs after):
- Error rate: 0.05% → 0.04% (improved ✓)
- Latency P95: 1.2s → 1.1s (improved ✓)
- Flag eval time: 12ms → 8ms (faster ✓)
- SDK size: -45KB (smaller bundle ✓)

Production Incidents: 0 ✓

Migration Complete!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Next Steps:
□ Cancel LaunchDarkly subscription
□ Archive LaunchDarkly export (backup)
□ Update team documentation
□ Conduct CloudBees FM training
□ Celebrate successful migration! 🎉

Cost Savings: $X/month
Performance: Improved
Risk: Zero incidents
Team: Trained on new platform ✓
```

## Required Skills

### Core Skills
- [ ] **platform-analyzer** - Assesses source platform
  - Identify source platform
  - Connect via API
  - Count and categorize flags
  - Assess migration complexity

- [ ] **flag-exporter** - Exports from source platform
  - Call source platform API
  - Export all flag data
  - Preserve metadata
  - Handle rate limits

- [ ] **config-mapper** - Maps between platforms
  - Translate flag configurations
  - Map flag types
  - Convert targeting rules
  - Preserve rollout states

- [ ] **migration-planner** - Creates migration strategy
  - Determine migration approach
  - Create phased plan
  - Define rollback procedures
  - Set success criteria

- [ ] **code-migrator** - Updates code references
  - Generate migration scripts
  - Update SDK imports
  - Replace API calls
  - Update flag names

- [ ] **parity-validator** - Ensures feature parity
  - Compare flag states
  - Validate targeting rules
  - Check rollout percentages
  - Monitor metrics

### Supporting Skills
- [ ] **bulk-flag-importer** - Imports flags to CloudBees FM
- [ ] **dual-run-configurator** - Sets up parallel SDKs
- [ ] **codemod-generator** - Creates automated code transformations
- [ ] **rollback-executor** - Handles migration rollbacks
- [ ] **training-content-generator** - Creates team training materials

## Constraints & Guardrails

**Must NOT do:**
- Delete source platform flags until migration complete
- Switch production without validation
- Lose flag configurations during migration
- Skip rollback planning
- Rush migration without testing

**Must ALWAYS do:**
- Validate feature parity
- Test in non-prod first
- Keep source platform running during migration
- Create backups/exports
- Monitor metrics before/after
- Provide rollback capability
- Document migration process
- Train team on new platform

## Edge Cases

1. **Case:** Source platform has features CloudBees doesn't support
   **Handling:** Identify unsupported features, find workarounds, document limitations

2. **Case:** Complex custom targeting rules
   **Handling:** Manual translation, validation with source team, test thoroughly

3. **Case:** Hundreds of code repositories
   **Handling:** Automate code updates, staged rollout per repo, parallel execution

4. **Case:** Active experiments mid-migration
   **Handling:** Pause experiments, migrate, resume, or migrate experiment data

5. **Case:** Source platform API rate limits
   **Handling:** Batch requests, add delays, request limit increase

6. **Case:** Different SDK architectures
   **Handling:** Major code refactoring, consider feature parity over code similarity

7. **Case:** Migration needs to happen quickly (contract ending)
   **Handling:** Accelerated timeline, more resources, accept higher risk

## Success Metrics
- [ ] Zero downtime during migration: 100%
- [ ] Feature parity achieved: 100%
- [ ] Flags migrated: 100%
- [ ] Code updated: 100% of repositories
- [ ] Production incidents: 0
- [ ] Migration timeline: Within planned schedule
- [ ] Cost savings: As projected
- [ ] Team satisfaction: 4+ stars

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Major undertaking: Requires careful planning
- Involves: Most other journeys (setup, testing, rollout, monitoring)
- Outcome: New platform ready for all standard workflows
