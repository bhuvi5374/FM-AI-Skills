**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I need step-by-step guidance to create my first feature flag in CloudBees FM, including naming it correctly, choosing the right type, and setting appropriate defaults.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > Create New Flag** form page in the Unify FM UI and asks "How do I fill out this form?"
2. Assistant reads the form fields visible on the page: Name field, Type selector (Boolean, String, Number), Description field, Tags field, and environment default value settings for each environment listed
3. Assistant calls `flags_configurations_list` to check for any existing flags with similar names, so it can warn against duplicates before the user submits
4. Assistant guides the user through each field one at a time:
   - Name: recommends a name using snake_case conventions (e.g., show_new_dashboard_button) and checks `flags_configurations_list` results for conflicts
   - Type: recommends Boolean for a first flag and explains why
   - Description: explains what to write here and helps draft one
   - Tags: suggests relevant tags (e.g., feature area, team)
5. Assistant configures default values per environment shown in the form (Local and Dev: ON for easy testing, Staging and Prod: OFF for safety)
6. User reviews the completed form and clicks "Create Flag" — assistant confirms the flag is created when the page transitions to the flag detail view

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > Create New Flag (form)
- **Page content to use:** All form fields on the Create New Flag page — Name input, Type selector (Boolean/String/Number), Description input, Tags input, and the per-environment default value toggles listed in the form
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to check for duplicate or similar flag names before the user submits the form

**Example Prompts to trigger skill**

- "Walk me through creating a flag in FM"
- "Help me fill out the flag creation form"
- "What should I name my flag?"
- "How do I set default values for my flag?"

**Constraints**

- Must start with the simplest flag type (boolean) for first-time users
- Must not rush through form fields — explain each one visible on the page
- Must always set production default to OFF
- Must check `flags_configurations_list` for naming conflicts before the user submits
- Must confirm creation succeeded when the page transitions to the flag detail view

**Acceptance Criteria**

The user is on the Create New Flag form page with all fields filled in correctly. `flags_configurations_list` has been checked for naming conflicts. The user's first flag is created with a clear name, description, tags, and appropriate environment defaults (Local and Dev ON, Staging and Prod OFF). The page has transitioned to the flag detail view confirming creation. The user understands what each field means.
