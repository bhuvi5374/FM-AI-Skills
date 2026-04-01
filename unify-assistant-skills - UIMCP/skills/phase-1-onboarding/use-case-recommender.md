**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I'm new to feature flags and need help choosing a simple, safe use case for my first flag so I can learn without risk.

**Conversation Flow (Happy Path)**

1. User is on the **Flags** main list page in the Unify FM UI and asks "Help me create my first feature flag" or "I'm new to FM, where do I start?"
2. Assistant reads the flags list visible on the page: how many flags currently exist, any existing flag names shown (to understand the team's naming patterns and existing usage)
3. Assistant calls `flags_configurations_list` to retrieve the full list of existing flags and check whether any similar flags already exist
4. Assistant asks what technology the user is working in (frontend, backend, language)
5. Assistant asks if the user has a feature in mind or wants a suggestion
6. If the user has a feature, assistant evaluates if it is a good starting candidate (simple, low-risk, visible result) and checks `flags_configurations_list` results for any duplicates
7. If the user wants a suggestion, assistant recommends a simple boolean use case relevant to their domain (e.g., "show/hide a new UI button" for frontend, "enable a new API endpoint" for backend)
8. User confirms the use case and the assistant tells them to click "Create Flag" on the current Flags list page to proceed

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags (main list page)
- **Page content to use:** The list of existing flags shown on the page — flag names, types, and the "Create Flag" button
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to check for existing or duplicate flags before recommending a use case

**Example Prompts to trigger skill**

- "Help me create my first feature flag"
- "I'm new to FM, how do I get started?"
- "What should my first flag be?"
- "Give me a simple example flag to start with"

**Constraints**

- Must always recommend a boolean (ON/OFF) flag for first-time users — never suggest multi-variant as a first flag
- Must not suggest use cases that touch payments, authentication, or other high-risk areas for a first flag
- Must check `flags_configurations_list` for duplicates before recommending a use case
- Must set realistic expectations about tutorial scope (flag creation, code integration, local testing only)

**Acceptance Criteria**

The user is on the Flags list page. `flags_configurations_list` has been called to check existing flags. The user has agreed on a simple, safe use case for their first feature flag. The use case is relevant to their domain, uses a boolean flag type, and the user is ready to click "Create Flag" to begin.
