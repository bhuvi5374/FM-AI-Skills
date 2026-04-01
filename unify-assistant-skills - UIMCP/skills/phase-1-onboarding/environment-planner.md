**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I'm new to CloudBees FM and need to know how many environments to create and what structure makes sense for my team's deployment pipeline.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Environments** page in the Unify FM UI and asks "How should I set up my environments?"
2. Assistant reads the current state of the page: what environments already exist (if any) shown in the list, and whether the list is empty (new account)
3. Assistant calls `flags_environments_list` to retrieve the current environment list and confirm what is already configured
4. Assistant asks about the user's deployment stages ("What environments does your team deploy to?")
5. User describes their pipeline (e.g., local, dev, staging, production)
6. Assistant recommends an environment structure matching that pipeline, explains the purpose of each environment, and suggests naming conventions based on what `flags_environments_list` returned (to avoid duplicates with any existing environments)
7. Assistant explains how flags behave differently per environment (e.g., dev = ON for testing, prod = OFF by default)
8. User confirms the recommended structure and the assistant tells them to click "Add Environment" on the current page to begin creating each one

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Environments
- **Page content to use:** The current list of environments shown on the page (names, types, order), and the "Add Environment" button
- **Unify MCP Server:** `flags_environments_list` (params: applicationId) — used to retrieve the existing environment list so recommendations avoid duplicates and account for what is already set up

**Example Prompts to trigger skill**

- "Help me set up my environments"
- "How many environments should I create?"
- "I need to configure dev, staging, and prod"
- "Set up environment structure for my team"

**Constraints**

- Must not recommend more than 6 environments without explaining the management overhead
- Must not skip explaining what each environment is for
- Must tailor recommendation to the user's actual pipeline, not a generic template
- Must cross-reference `flags_environments_list` results to avoid recommending duplicates

**Acceptance Criteria**

The user is on the Settings > Environments page. `flags_environments_list` has been called to check existing environments. The user has a clear, agreed-upon environment structure with names and purposes defined, understands how flag behavior differs per environment, and is positioned to click "Add Environment" to begin creating the planned environments.
