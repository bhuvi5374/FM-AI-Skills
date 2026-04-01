**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to configure the right default flag behaviors and approval settings for each environment so that flags work safely across dev, staging, and production.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Environments > (environment) > Default Settings** page in the Unify FM UI and asks "How should I configure defaults for this environment?"
2. Assistant reads the environment name and the settings visible on the page: default flag value toggle (ON/OFF), rollout policy selector, approval requirement toggle
3. Assistant calls `flags_environments_list` to confirm which environments exist and determine which tier the current environment belongs to (dev vs staging vs prod)
4. Assistant recommends the appropriate defaults for the current environment tier: dev = flags ON for easy testing, prod = flags OFF with manual rollout required
5. Assistant walks the user through setting each visible field on the page: default flag value, rollout policy, approval requirements
6. For production specifically, assistant warns before recommending OFF + manual + approval required settings and explains the safety reason
7. User adjusts each setting on the page and saves — assistant confirms the settings look correct based on what the user reports is now shown

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Environments > (environment) > Default Settings
- **Page content to use:** The environment name shown at the top of the page, default flag value toggle (ON/OFF), rollout policy selector options, approval requirement toggle, and the "Save" button
- **Unify MCP Server:** `flags_environments_list` (params: applicationId) — used to see all environments and determine the context/tier of the current environment being configured

**Example Prompts to trigger skill**

- "Configure flag defaults for each environment"
- "Set production flags to OFF by default"
- "Require approvals for production rollouts"
- "Set up rollout policies"

**Constraints**

- Must not set production to default ON without explicit user confirmation and warning
- Must always explain the security implications of each setting before the user changes it
- Must configure approval requirements for production
- Must not skip production safety configuration
- Must reference the specific toggle and selector values visible on the Default Settings page

**Acceptance Criteria**

The user is on the Default Settings page for the environment being configured. Each setting shown on the page (default flag value, rollout policy, approvals) has been reviewed and set appropriately. `flags_environments_list` was used to understand the environment tier. The page shows the saved configuration and the user understands the impact of the settings.
