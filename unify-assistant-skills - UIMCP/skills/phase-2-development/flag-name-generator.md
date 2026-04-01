**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I need a well-structured, consistent flag name that follows my team's naming conventions and avoids conflicts with existing flags.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > Create New Flag** page in the Unify FM UI with the Name field focused and asks "What should I name this flag?"
2. Assistant reads any name already entered in the Name field on the page (if the user has started typing)
3. Assistant calls `flags_configurations_list` to retrieve all existing flag names and detect naming patterns the team already uses (e.g., feature_ prefix, experiment_ prefix, snake_case)
4. Assistant suggests a flag name based on the team's observed naming patterns and the feature description
5. Assistant checks the suggested name against the `flags_configurations_list` results for conflicts
6. Assistant provides 2–3 alternative names for the user to choose from
7. User selects or modifies the name and types it into the Name field on the page
8. Assistant validates the chosen name (no spaces, no special characters, no ambiguous abbreviations) before the user proceeds

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > Create New Flag (name field focused)
- **Page content to use:** The Name input field on the Create New Flag form — any value the user has already typed, and any inline validation messages shown by the field
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve existing flag names for conflict detection and to infer the team's naming conventions

**Example Prompts to trigger skill**

- "Suggest a name for my flag"
- "What should I call this feature flag?"
- "Generate a flag name for dark mode"
- "Check if my flag name conflicts with existing ones"

**Constraints**

- Must check for naming conflicts against all flags returned by `flags_configurations_list`
- Must follow naming convention (snake_case, meaningful prefix) inferred from existing flags
- Must not accept names with spaces, special characters, or ambiguous abbreviations
- Must offer alternatives if the first suggestion is rejected

**Acceptance Criteria**

The user is on the Create New Flag page with the Name field focused. `flags_configurations_list` has been checked for conflicts and naming patterns. The flag name entered in the Name field follows the team's conventions, has no conflicts with existing flags, clearly describes the feature or experiment it controls, and has been confirmed by the user.
