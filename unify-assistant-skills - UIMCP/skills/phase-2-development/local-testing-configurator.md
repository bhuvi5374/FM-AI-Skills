**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to set up my local development environment so I can easily toggle my feature flag between ON and OFF states without having to go back and forth to the FM UI during testing.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Configuration > Local environment** view in the Unify FM UI and asks "How do I set up fast local testing for this flag?"
2. Assistant reads the flag's Local environment configuration visible on the page: current value (ON/OFF), any override rules shown, and the environment toggle/controls
3. Assistant calls `flags_configurations_list` to retrieve the flag's full configuration and confirm the Local environment's current value and type
4. Assistant presents three override options to the user: (1) use the toggle on the current Configuration page in the FM UI, (2) use an environment variable, (3) use the SDK override API in code
5. Assistant recommends the SDK override API for fast automated testing and provides the exact code snippet for the override in the user's language
6. Assistant also explains how to use the toggle on the current Configuration > Local page for quick manual toggling
7. Assistant helps the user set up test user contexts for testing different targeting scenarios
8. User confirms the setup works and the assistant explains how to remove the override after testing is complete

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Configuration > Local environment
- **Page content to use:** The Local environment value toggle (ON/OFF) on the Configuration page, current value shown, any targeting rule overrides visible for the Local environment
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the flag's full configuration including the Local environment's current value and flag type

**Example Prompts to trigger skill**

- "How do I toggle this flag locally without using the FM UI?"
- "Set up local flag override for testing"
- "Show me how to test the ON and OFF state quickly"
- "Configure my local test environment for this flag"

**Constraints**

- Must present all available override options (UI toggle, environment variable, SDK override), not just one
- Must recommend SDK override for automated tests but always mention the UI toggle on the current page for manual testing
- Must warn that SDK override code should never be committed to production
- Must include how to reset/remove the override after testing (including resetting the page toggle)

**Acceptance Criteria**

The user is on the flag's Configuration > Local environment page. `flags_configurations_list` has confirmed the flag configuration. The user understands all three override options, has the SDK override code snippet if needed, and knows how to use the toggle on the current page for quick manual toggling. They know how to reset after testing.
