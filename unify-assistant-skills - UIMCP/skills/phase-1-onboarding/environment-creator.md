**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to create my environments in CloudBees FM and generate unique SDK keys for each one so my team can start integrating feature flags.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Environments > Add Environment** page in the Unify FM UI and asks "How do I create a new environment?"
2. Assistant reads the form fields visible on the Add Environment page: Name field, Description field, Type selector, Color/icon selector (if shown)
3. Assistant guides the user through filling out the form for each environment in the planned structure (e.g., Local, Development, Staging, Production) — one at a time
4. For each environment: assistant tells the user which values to enter in each visible form field, then instructs the user to click "Create" or "Save"
5. After each environment is created, the user is returned to the Settings > Environments list — assistant calls `flags_environments_list` to confirm the new environment appears
6. Once all environments are created, assistant confirms the full list and tells the user where to find SDK keys (Settings > Environments > (environment) > SDK Keys)

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Environments > Add Environment
- **Page content to use:** Form fields on the Add Environment page — Name input, Description input, Type selector, and the "Create" / "Save" button
- **Unify MCP Server:** `flags_environments_list` (params: applicationId) — called after each environment is created to verify it appears in the list

**Example Prompts to trigger skill**

- "Create my environments in FM"
- "Set up dev, staging, and production environments"
- "Generate SDK keys for each environment"

**Constraints**

- Must validate environment names follow naming conventions before the user submits the form
- Must not create duplicate environments — use `flags_environments_list` to check before each creation
- Must guide the user through the form for each environment one at a time
- Must confirm each creation via `flags_environments_list` before proceeding to the next

**Acceptance Criteria**

All planned environments are created through the Add Environment form. `flags_environments_list` confirms each environment appears after creation. The user can see all environments listed on the Settings > Environments page and knows where to access SDK keys for each one.
