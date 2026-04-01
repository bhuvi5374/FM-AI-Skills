**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to configure my new feature flag with the right environment defaults, description, tags, Jira link, and rollout strategy so it is ready to use from day one.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > Create New Flag** full form page in the Unify FM UI and asks "Help me configure all the settings for this flag"
2. Assistant reads all visible form fields on the page: Name (already set), Type (already set), Description field, Tags field, Jira issue link field, and the per-environment default value settings listed for each environment
3. Assistant calls `flags_configurations_list` to check existing flags for configuration patterns (tags, descriptions) to inform recommendations
4. Assistant calls `flags_environments_list` to retrieve the full list of configured environments and their order, so it can recommend the right default value for each environment shown in the form
5. Assistant guides the user through each remaining field:
   - Description: helps draft one including the Jira ticket reference if available
   - Tags: suggests relevant tags (feature area, quarter, experiment type)
   - Jira link: instructs the user to enter the Jira ticket URL in the Jira field on the form (if integration is configured)
   - Per-environment defaults: recommends Local and Dev = ON for testing, Staging and Prod = OFF, matched to the environments returned by `flags_environments_list`
   - Rollout strategy: sets gradual rollout template (0% → 10% → 50% → 100%) and approval requirements for production
6. User reviews the completed form and confirms — assistant tells the user to click "Create" or "Save"

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > Create New Flag (full form)
- **Page content to use:** All form fields — Name, Type, Description, Tags, Jira link, and per-environment default value toggles for each environment listed in the form
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — to check existing flag patterns; `flags_environments_list` (params: applicationId) — to retrieve the full environment list and match default value recommendations to each environment in the form

**Example Prompts to trigger skill**

- "Configure the flag settings"
- "Set up environment defaults for my flag"
- "Add tags and link to my Jira ticket"
- "Configure rollout strategy for this flag"

**Constraints**

- Must not set production default to ON without explicit user confirmation and a clear warning
- Must always recommend approval requirement for production rollout
- Must link to Jira ticket if one has been provided
- Must not skip adding a description — flags without descriptions cause confusion
- Must match environment default recommendations to the actual environments returned by `flags_environments_list`

**Acceptance Criteria**

The user is on the Create New Flag form with all fields completed. `flags_environments_list` has been called to match recommendations to the actual environments shown in the form. The flag form is fully configured: description with Jira reference, relevant tags, correct per-environment defaults, rollout strategy, and production approval requirement. The user has confirmed the configuration and the form is ready to submit.
