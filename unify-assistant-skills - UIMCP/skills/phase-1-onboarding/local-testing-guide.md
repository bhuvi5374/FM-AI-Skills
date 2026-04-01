**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I've added a feature flag to my code and need to test it locally to make sure both the ON state (new feature) and OFF state (original behavior) work correctly before deploying.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Configuration > Local environment** view in the Unify FM UI and asks "How do I test this flag locally?"
2. Assistant reads the flag's Local environment configuration visible on the page: current value (ON or OFF), any override settings shown, and the environment status indicator
3. Assistant calls `flags_configurations_list` to retrieve the flag's full configuration and confirm the Local environment's default value
4. Assistant guides the user to start their app locally and verify the SDK connected (check console logs for "Connected to CloudBees FM" and flag value loaded)
5. Assistant walks through testing the ON state: the page already shows Local = ON (or the user sets it via the toggle visible on the page), user verifies the new feature appears in their running app
6. Assistant walks through testing the OFF state: user sets the Local environment toggle to OFF on the current page, refreshes their app, and verifies original behavior returns
7. Both states pass — assistant confirms the flag is working correctly based on the page toggle responding and the app reflecting the changes

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Configuration > Local environment
- **Page content to use:** The Local environment flag value toggle (ON/OFF) on the Configuration page, current value indicator, and any override or targeting rules shown for the Local environment
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the flag's full configuration and confirm the Local environment's current value setting

**Example Prompts to trigger skill**

- "How do I test this flag locally?"
- "Help me test the ON and OFF state of my flag"
- "Verify my flag is working before I deploy"
- "Test my feature flag implementation"

**Constraints**

- Must test both ON and OFF states using the toggle visible on the Configuration page — must not declare success after testing only one state
- Must help the user interpret SDK console logs to confirm connection
- Must not suggest testing in production or staging for a first flag
- Must provide troubleshooting steps if the flag does not appear to respond to the page toggle

**Acceptance Criteria**

The user is on the flag's Configuration page for the Local environment. `flags_configurations_list` has confirmed the flag configuration. The user has toggled the flag ON and OFF using the controls on the page and verified both states work in their running local app. The new feature appears when the flag is ON and is hidden when the flag is OFF.
