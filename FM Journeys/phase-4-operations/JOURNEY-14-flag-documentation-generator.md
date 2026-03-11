# Journey: Flag Documentation Generator

## Overview
**Goal:** Automatically generate and maintain comprehensive documentation for feature flags to improve team knowledge and onboarding.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Generate documentation for all my flags"
- "Create docs for this flag"
- "I need flag documentation for the team"
- "Auto-document our feature flags"

**Success Criteria:**
- Documentation generated for selected flags
- All key information included
- Documentation kept in sync with changes
- Accessible to entire team
- Improves team understanding
- Reduces onboarding time

## User Persona
- **Role:** Engineering Manager, Tech Writer, Developer, Product Manager
- **Pain Point:**
  - Flags created without documentation
  - Hard for new team members to understand flags
  - Context lost over time (why flag was created)
  - Manually writing docs is tedious
  - Documentation gets out of date
  - Need to explain flags repeatedly

- **Current Manual Process:**
  1. Write docs manually (if at all)
  2. Copy-paste flag details
  3. Documentation lives in random places
  4. Becomes outdated immediately
  5. No one maintains it

  Time: 30-60 minutes per flag, often skipped

## Agentic Happy Path

### Step 1: Select Flags to Document
**What the agent does:**
- Ask which flags need documentation
- Identify undocumented flags
- Prioritize by importance/usage
- Allow bulk or individual selection

**Context needed:**
- All flags in system
- Existing documentation status
- Flag metadata

**Example:**
```
Agent: "Which flags should I document?"

Options:
1. All flags (127 total)
2. Undocumented flags only (43 flags)
3. Specific flag
4. Flags created in last 30 days

User: "Undocumented flags only"

Agent: "Found 43 undocumented flags.
Top 10 by usage:
1. feature_new_checkout_page (2M impressions/day)
2. rollout_dashboard_v2 (800K impressions/day)
3. experiment_pricing_test (500K impressions/day)
...

Generate docs for all 43 or start with top 10?"
User: "Top 10 first"
```

### Step 2: Gather Flag Information
**What the agent does:**
- Collect flag metadata
- Pull rollout history
- Get code references
- Find linked Jira tickets
- Review commits mentioning flag
- Extract business context

**Context needed:**
- CloudBees FM API access
- Git history
- Jira integration
- Code scanning results

**Example:**
```
Gathering info for: feature_new_checkout_page

Basic Info:
- Created: 90 days ago by Alice Chen
- Type: Boolean
- Current status: 75% rollout in production

Code References:
- src/checkout/CheckoutPage.tsx (5 references)
- src/checkout/payment.ts (2 references)
- tests/checkout.test.ts (8 test cases)

Jira Ticket:
- SHOP-456: "Redesign checkout page"
  Status: Done
  Description: "Modernize checkout UX,
  reduce abandonment rate"

Git History:
- Initial commit: "Add flag for new checkout"
  (commit abc123, 90 days ago)
- 12 commits mention this flag
- Last modified: 7 days ago

Performance Data:
- Impressions: 2.4M in last 30 days
- Conversion: +14% vs baseline
- Errors: 0.04% (improved from 0.08%)
```

### Step 3: Generate Documentation
**What the agent does:**
- Create structured documentation
- Include purpose and business context
- Document technical implementation
- Add rollout history
- Include usage examples
- Provide troubleshooting tips

**Context needed:**
- Documentation template
- Team documentation standards
- Target audience (developers, PM, etc.)

**Example:**
```
Generated Documentation:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Feature Flag: feature_new_checkout_page
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Overview
Controls the display of the redesigned checkout page.
When enabled, users see the new streamlined checkout UX.
When disabled, users see the original checkout flow.

**Status:** Active (75% rollout in production)
**Owner:** Alice Chen (@alice)
**Created:** December 18, 2025
**Jira:** SHOP-456

## Business Context
The new checkout page was designed to reduce cart
abandonment and improve conversion rates. User research
showed the old checkout had too many steps. The new
design streamlines the process from 4 steps to 2.

**Goals:**
- Reduce cart abandonment by 15%
- Improve conversion rate
- Modern, mobile-friendly design

## Technical Implementation

### Flag Type
Boolean (true = new checkout, false = old checkout)

### Code Locations
- `src/checkout/CheckoutPage.tsx` - Main UI component
- `src/checkout/payment.ts` - Payment processing logic
- `tests/checkout.test.ts` - Test coverage

### Usage Example
```typescript
import { useFlag } from '@cloudbees/fm-react-sdk';

function CheckoutPage() {
  const showNewCheckout = useFlag('feature_new_checkout_page');

  return showNewCheckout ? (
    <NewCheckoutPage />
  ) : (
    <LegacyCheckoutPage />
  );
}
```

## Rollout History
- **Dec 18**: Created, tested in staging
- **Jan 5**: Rolled to 1% production (beta users)
- **Jan 12**: Increased to 10%
- **Jan 26**: Increased to 25%
- **Feb 9**: Increased to 50%
- **Mar 4**: Increased to 75% (current)

## Performance
**Metrics (Last 30 days at 75% rollout):**
- Impressions: 2.4M
- Conversion rate: 4.8% (+14% vs baseline)
- Error rate: 0.04% (improved from 0.08%)
- P95 latency: 1.2s (improved from 1.5s)
- Cart abandonment: 42% (down from 51%)

✓ All metrics show improvement

## Environment Configuration
- **Local:** 100% ON (for testing)
- **Development:** 100% ON
- **Staging:** 100% ON
- **Production:** 75% rollout (active)

## Target Groups
Currently: Random 75% of all users
Future: May target premium users first

## Dependencies
- Requires: feature_new_payment_processor (must be ON)
- Related: experiment_one_step_checkout (experimental variation)

## Testing
- Unit tests: 92% coverage
- Integration tests: 15 tests
- E2E scenarios: 8 automated tests
- Manual test checklist: [Link to QA doc]

## Troubleshooting

**Issue:** Users report not seeing new checkout
**Fix:** Verify user is in the 75% rollout group.
Check SDK logs for flag evaluation.

**Issue:** Payment fails on new checkout
**Fix:** Ensure feature_new_payment_processor is ON.
Check compatibility with payment provider.

## Next Steps
- **Short term:** Increase to 100% after 1 week stable at 75%
- **Long term:** Remove old checkout code, clean up flag
  (estimated 2 weeks after 100% rollout)

## Resources
- Jira: [SHOP-456](link)
- Design specs: [Figma](link)
- A/B test results: [Dashboard](link)
- Team discussion: [Slack thread](link)

---
*Auto-generated on March 18, 2026*
*Last updated: March 18, 2026*
```

### Step 4: Format & Organize Documentation
**What the agent does:**
- Choose documentation format (Markdown, Wiki, etc.)
- Organize by categories or teams
- Add table of contents
- Create navigation/links
- Apply team templates

**Context needed:**
- Team documentation system
- Preferred format
- Organization structure

**Example:**
```
Documentation Output Options:

1. Markdown files (Git repo)
   Location: /docs/feature-flags/
   Format: feature_new_checkout_page.md

2. Confluence Wiki
   Space: Engineering Docs
   Page: Feature Flags / Active Flags

3. Notion
   Database: Feature Flags
   Entry: New row per flag

4. Google Docs
   Folder: Feature Management
   Doc: One per flag

User: "Markdown in Git repo"

Created:
✓ /docs/feature-flags/feature_new_checkout_page.md
✓ /docs/feature-flags/README.md (index)
✓ Git commit: "Add docs for feature_new_checkout_page"
```

### Step 5: Keep Documentation in Sync
**What the agent does:**
- Set up auto-update triggers
- Notify when flag changes
- Suggest doc updates
- Generate changelog
- Archive old flags

**Context needed:**
- Change notification system
- Update frequency preferences
- Automation capabilities

**Example:**
```
Documentation Maintenance Setup:

Auto-Update Triggers:
✓ Flag rollout percentage changes
✓ Flag configuration updates
✓ New code references detected
✓ Performance metrics (weekly summary)

Notifications:
- Slack: Post to #documentation when flag docs outdated
- Email: Weekly summary of documentation changes
- PR Comments: Remind to update docs when flag changes

Suggested Updates:
"feature_new_checkout_page rolled to 100%!"
→ Update documentation with new status
→ Add to "Ready for cleanup" section

Archiving:
Flags deleted from FM → Move docs to archive/
Retain for historical reference
```

## Required Skills

### Core Skills
- [ ] **flag-selector** - Identifies flags needing docs
  - List all flags
  - Check existing documentation
  - Prioritize by importance
  - Allow filtering/selection

- [ ] **info-gatherer** - Collects flag information
  - Query FM API for metadata
  - Get code references
  - Pull Jira tickets
  - Review git history
  - Gather performance data

- [ ] **doc-generator** - Creates documentation
  - Apply documentation template
  - Structure information clearly
  - Include all relevant sections
  - Format for readability
  - Add examples and diagrams

- [ ] **doc-formatter** - Formats and organizes docs
  - Convert to target format (MD, Wiki, etc.)
  - Apply team styles
  - Create navigation
  - Add links and references

- [ ] **doc-sync-manager** - Keeps docs updated
  - Detect flag changes
  - Trigger update notifications
  - Generate changelogs
  - Archive deleted flags

### Supporting Skills
- [ ] **template-customizer** - Adapts doc templates per team
- [ ] **diagram-generator** - Creates visual diagrams
- [ ] **changelog-generator** - Tracks doc changes over time
- [ ] **search-indexer** - Makes docs searchable

## Constraints & Guardrails

**Must NOT do:**
- Include sensitive information (API keys, credentials)
- Make assumptions about business context without data
- Generate docs without verifying information
- Overwrite manually added documentation sections
- Skip critical sections (purpose, usage, troubleshooting)

**Must ALWAYS do:**
- Verify all information is accurate
- Include code examples
- Provide business context
- Add troubleshooting section
- Link to related resources
- Include contact/owner info
- Date documentation
- Respect team documentation standards

## Edge Cases

1. **Case:** Flag has no Jira ticket or business context
   **Handling:** Generate technical docs, prompt user to add business context

2. **Case:** Flag is experimental with uncertain future
   **Handling:** Mark as experimental, note temporary nature

3. **Case:** Flag has been modified many times
   **Handling:** Summarize change history, link to full audit log

4. **Case:** Flag is security-sensitive
   **Handling:** Mark as sensitive, limit documentation detail, restrict access

5. **Case:** Team uses custom documentation system
   **Handling:** Provide raw content, offer integration guidance

6. **Case:** Multiple flags for same feature
   **Handling:** Create master doc linking related flags

7. **Case:** Flag documentation exists but outdated
   **Handling:** Update existing doc, preserve custom sections

## Success Metrics
- [ ] Documentation generation time: <2 minutes per flag
- [ ] Documentation completeness: 90%+ sections filled
- [ ] Team finds docs useful: 4+ stars
- [ ] Onboarding time reduced: 30%+
- [ ] "Why does this flag exist?" questions: -50%
- [ ] Documentation accuracy: 95%+
- [ ] Docs stay up-to-date: 85%+ within 1 week of changes

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Works with: JOURNEY-11-flag-health-audit (documents audit findings)
- Complements: JOURNEY-05-new-flag-setup-wizard (create docs at creation)
- Improves: Team knowledge and onboarding
