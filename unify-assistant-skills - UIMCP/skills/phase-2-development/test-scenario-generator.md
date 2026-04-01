**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I need a comprehensive list of test scenarios for my feature flag — covering both ON and OFF states, edge cases, and targeting scenarios — so I don't miss anything before deploying.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Overview + Configuration tabs** in the Unify FM UI and asks "What should I test for this flag?"
2. Assistant reads the flag information visible on the Overview tab: flag name, type, description, and any Jira ticket link shown
3. Assistant reads the Configuration tab to see the environment defaults and any targeting rules configured for each environment
4. Assistant calls `flags_configurations_list` to retrieve the flag's full configuration details
5. Assistant calls `flags_environments_list` to see all configured environments and determine which ones the flag is deployed to
6. Assistant generates a test checklist for the ON state: critical flows, error handling, edge cases based on the code references identified in flag-impact-analyzer
7. Assistant generates a test checklist for the OFF state: original behavior intact, no regressions
8. Assistant adds state transition scenarios: toggle ON → OFF → ON during an active session (using the Configuration page controls)
9. Assistant adds targeting scenarios: test as a user in the target group vs a user outside it (if targeting rules are shown on the Configuration tab)
10. User works through the checklist and the assistant marks items complete as confirmed

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Overview + Configuration tabs
- **Page content to use:** Flag name, type, and description from the Overview tab; environment default values, rollout percentages, and targeting rules from the Configuration tab
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — to retrieve full flag configuration details; `flags_environments_list` (params: applicationId) — to retrieve all environments and determine which ones the flag is deployed to

**Example Prompts to trigger skill**

- "What should I test for this flag?"
- "Generate a test checklist for ON and OFF states"
- "What edge cases should I check?"
- "Create test scenarios for this flag"

**Constraints**

- Must always generate scenarios for BOTH ON and OFF states — never just the ON state
- Must include at least one state transition scenario
- Must flag critical paths (payments, auth) for mandatory testing based on code references
- Must not skip edge cases just because they seem unlikely
- Must base environment-specific scenarios on the environments returned by `flags_environments_list`

**Acceptance Criteria**

The user is on the flag's Overview and Configuration tabs. Both `flags_configurations_list` and `flags_environments_list` have been called. The user has a complete test checklist covering ON state, OFF state, state transitions, and targeting scenarios — all grounded in the actual flag configuration and environments shown on the page. Critical user flows are clearly marked. The user can work through the checklist to confirm all scenarios pass before deployment.
