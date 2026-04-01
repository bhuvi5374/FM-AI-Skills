**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to actually create the feature flag in CloudBees FM so it is live and accessible to the SDK immediately.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > Create New Flag** page with the form fully filled out and asks "OK, create the flag now" or clicks the submit button
2. Assistant reads the form values visible on the page one final time: flag name, type, description, tags, environment defaults — to confirm all required fields are present before submission
3. Assistant calls `flags_configurations_list` to do a final duplicate-name check before the flag is created
4. If no conflicts exist, assistant instructs the user to click the "Create" button on the page (or confirms the user is proceeding with the submit)
5. The page transitions to the newly created flag's detail view — assistant reads the flag name, flag ID, and status shown on the new flag detail page to confirm creation was successful
6. Assistant calls `flags_configurations_list` again to verify the new flag now appears in the flag list
7. Assistant shows the user they are now on the flag detail page and explains the direct URL they can share with the team
8. Assistant tells the user to remain on this page and proceed to team notification (if Slack is configured) or code snippet generation

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > Create New Flag > (submit) → transitions to new flag detail page
- **Page content to use:** The completed form fields before submission (flag name, type, description, environment defaults), and the flag detail page content after creation (flag name, flag ID, status indicator)
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — called before submission for a final duplicate check, and called again after creation to verify the flag appears in the list. REST API fallback: POST /v2/applications/{appId}/flags — used to create the flag programmatically if the user is using the MCP-driven workflow rather than the UI form directly

**Example Prompts to trigger skill**

- "Create the flag now"
- "Save the flag to CloudBees FM"
- "Make the flag live"

**Constraints**

- Must not submit without all required fields confirmed (name, type, environment defaults)
- Must do a final `flags_configurations_list` duplicate check before submission
- Must verify creation by calling `flags_configurations_list` after the page transitions to the flag detail view
- Must handle creation errors gracefully and report them clearly

**Acceptance Criteria**

The Create New Flag form has been submitted. The page has transitioned to the new flag's detail view. `flags_configurations_list` confirms the flag now appears in the list. The user has the flag detail page open, sees the flag's name and status, and has a shareable link to the flag.
