**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I need a ready-to-use code snippet in my programming language so I can quickly integrate the new feature flag into my codebase without looking up SDK documentation.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Overview tab** in the Unify FM UI with the flag name and type visible and asks "Can you generate the code for this flag?"
2. Assistant reads the flag name and flag type shown on the Overview tab of the flag detail page
3. Assistant calls `flags_configurations_list` to retrieve the full flag configuration — confirming the flag name, type, and environment defaults to ensure the generated snippet is accurate
4. Assistant asks for the user's programming language and framework (if not already known from conversation context)
5. Assistant generates a flag check code snippet using the correct SDK syntax for the user's language, using the exact flag name visible on the page
6. Assistant shows a usage example with the feature wrapped in the flag condition, explaining what happens in the ON path vs the OFF path
7. Assistant generates a basic test case template showing how to mock the flag in the user's testing framework
8. User receives the snippet ready to copy into their IDE — the flag name matches what is shown on the Overview tab

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Overview tab (flag name and type visible)
- **Page content to use:** The flag name and flag type displayed on the Overview tab
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the full flag configuration to ensure the generated code snippet uses the correct flag name and type

**Example Prompts to trigger skill**

- "Generate code snippet for this flag"
- "Show me how to use this flag in React"
- "Give me the Python SDK code for this flag"
- "What's the code to check this flag?"

**Constraints**

- Must use the exact flag name visible on the Overview tab and confirmed by `flags_configurations_list`
- Must generate code in the user's actual language — must not default to JavaScript without asking
- Must show both the flag check AND the feature code within context (not just the SDK import)
- Must include a test case template, not just the implementation code

**Acceptance Criteria**

The user is on the flag's Overview tab. `flags_configurations_list` has confirmed the flag name and type. The user receives a correct, ready-to-use code snippet in their programming language that checks the flag using the exact name shown on the page, wraps the feature in the correct condition, and includes a test case template.
