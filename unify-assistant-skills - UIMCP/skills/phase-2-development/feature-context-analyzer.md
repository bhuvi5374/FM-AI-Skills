**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to create a new feature flag but need help understanding the right flag type, scope, and setup approach based on the feature I'm building.

**Conversation Flow (Happy Path)**

1. User is on the **Flags** main list page in the Unify FM UI and says "I need to create a new flag for dark mode" or "Help me set up a flag for the checkout redesign"
2. Assistant reads the flags list visible on the page: existing flag names, types, and the total number of flags shown
3. Assistant calls `flags_configurations_list` to retrieve the full list of existing flags and check for duplicates or similar flags that might already cover the same feature
4. If a similar flag exists, assistant surfaces it to the user and asks whether they want to reuse it or create a new one
5. Assistant asks about the feature: what does it do, who will use it, is it frontend/backend/full-stack
6. Assistant determines the appropriate flag type: boolean (ON/OFF feature), string (multiple variants), number (configuration value)
7. Assistant identifies the rollout expectation: gradual percentage rollout, targeted segment, or simple ON/OFF
8. Assistant checks if a Jira ticket exists for this feature and if the Jira integration is configured
9. Assistant summarizes: flag type, rollout type, ticket reference, and instructs the user to click "Create Flag" on the current page to proceed

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags (main list page)
- **Page content to use:** The existing flags list showing flag names and types, and the "Create Flag" button on the page
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to check for existing duplicate or similar flags before the user begins creating a new one

**Example Prompts to trigger skill**

- "Create a new flag for dark mode feature"
- "I need a flag for the checkout redesign"
- "Set up a flag for A/B testing the new pricing page"
- "Help me create a flag quickly"

**Constraints**

- Must check `flags_configurations_list` for duplicate or similar flags before proceeding
- Must not assume flag type — determine it based on the feature description
- Must ask about rollout expectations to inform configuration
- Must surface any similar existing flags found in `flags_configurations_list` results

**Acceptance Criteria**

The user is on the Flags list page. `flags_configurations_list` has been called and checked for duplicates. The assistant has correctly determined the appropriate flag type (boolean, string, or number), rollout approach, and Jira ticket reference. The user has confirmed this understanding and is ready to click "Create Flag" to proceed.
