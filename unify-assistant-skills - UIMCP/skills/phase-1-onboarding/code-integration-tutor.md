**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I've created my first flag in CloudBees FM and now need help adding it to my code so the flag actually controls the feature in my application.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Overview tab** in the Unify FM UI with the flag name and type visible, and asks "How do I add this flag to my code?"
2. Assistant reads the flag name and flag type shown on the Overview tab of the flag detail page
3. Assistant calls `flags_configurations_list` to retrieve the full flag configuration, confirming the flag name, type, and environment defaults
4. Assistant asks for the user's programming language and framework (if not already known from conversation context)
5. Assistant generates a code snippet using the correct SDK for the user's language, using the exact flag name shown on the page
6. Assistant explains the if/else logic: when the flag is ON, show new feature; when the flag is OFF, show original behavior
7. Assistant suggests where in the code to place the flag check based on the feature description
8. Assistant confirms the code structure is correct and instructs the user to return to the flag Overview page to verify the flag is still active before testing locally

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Overview tab (flag name and type visible)
- **Page content to use:** The flag name and flag type shown on the Overview tab, current flag status indicator, and any SDK key reference shown on the page
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the full flag configuration including name, type, and environment defaults to generate accurate code snippets

**Example Prompts to trigger skill**

- "How do I add this flag to my code?"
- "Generate a code snippet for my React app"
- "Show me how to use the flag in my code"
- "How does the SDK work in Python?"

**Constraints**

- Must use the exact flag name visible on the Overview tab and confirmed via `flags_configurations_list`
- Must generate code in the correct language/framework for the user
- Must explain the if/else flag logic — not just provide code
- Must show both the ON path (new feature) and OFF path (fallback)
- Must not generate code with hardcoded flag values (always use SDK call)

**Acceptance Criteria**

The user is on the flag's Overview tab. `flags_configurations_list` has confirmed the flag name and type. The user has a ready-to-use SDK flag check code snippet in their programming language that uses the exact flag name from the page. The code handles both ON and OFF states and the user understands how the logic works.
