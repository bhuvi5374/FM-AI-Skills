**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

Before testing my feature flag, I want to understand exactly what the flag controls — which code paths are affected when it is ON vs OFF — so I know what to test.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Code References tab** in the Unify FM UI and asks "What does this flag actually control?" or "Which code paths are affected?"
2. Assistant reads the code reference entries visible on the Code References tab: file paths, line numbers, branch names, and code snippet previews shown for the flag
3. Assistant calls `flags_configurations_list` to retrieve the flag's full configuration — type, environment defaults, any targeting rules — to provide full context on the flag's impact
4. Assistant parses the code reference snippets visible on the page to identify the ON path (new feature behavior) and OFF path (original/fallback behavior)
5. Assistant identifies the critical user flows affected by this flag based on the file names and code snippets shown
6. Assistant categorizes the impact as critical or non-critical (e.g., if code references appear in checkout, payment, or auth files — flag as critical)
7. Assistant summarizes its findings and instructs the user to navigate to the flag's Configuration tab to proceed with local testing setup

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Code References tab
- **Page content to use:** The list of code reference entries on the tab — file paths, line numbers, branch names, and code snippet previews
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the flag's full configuration (type, environment values, targeting) to complement the code reference data shown on the page

**Example Prompts to trigger skill**

- "What does this flag control?"
- "Show me the impact of turning off this flag"
- "Which user flows are affected by this flag?"
- "Analyze what this flag does in my code"

**Constraints**

- Must identify both ON and OFF code paths using the snippets visible on the Code References tab — not just the new feature path
- Must flag critical user flows (e.g., checkout, authentication, payments) for extra testing attention
- Must not proceed to testing recommendations without identifying both states
- Must use code references visible on the tab — not guesswork

**Acceptance Criteria**

The user is on the flag's Code References tab. `flags_configurations_list` has been called for full configuration context. The assistant has identified all code locations shown on the tab, described what happens in both ON and OFF states based on the snippets, and flagged which user flows are critical. The user understands the full impact of the flag before testing begins.
