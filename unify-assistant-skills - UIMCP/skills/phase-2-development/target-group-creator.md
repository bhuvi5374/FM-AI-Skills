**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to save my targeting rules as a named target group in CloudBees FM so I can reuse it across multiple feature flags and rollouts.

**Conversation Flow (Happy Path)**

1. User is on the **Target Groups > Create New Target Group > Save** step in the Unify FM UI and asks "OK, save this target group"
2. Assistant reads the target group form fields visible on the page: Name field, Description field, and the conditions that have been configured
3. Assistant confirms the group name is descriptive (e.g., "beta_premium_users") and helps the user write a description documenting the group's purpose and conditions
4. Assistant instructs the user to click "Save" or "Create" on the current page
5. After saving, the page transitions to the target group detail view — assistant reads the group name and condition summary shown to confirm creation
6. Assistant uses REST POST /v2/applications/{appId}/target-groups if creating the group programmatically in the MCP-driven workflow; otherwise confirms the UI save was successful
7. Assistant shows the user how to test the group with a specific user ID using the test/preview tool on the target group detail page
8. Assistant explains how to apply this group to a flag rollout (Flags > (flag) > Targeting > Select group)

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Target Groups > Create New Target Group > Save
- **Page content to use:** The target group Name field, Description field, the conditions summary visible on the form, and the "Save" / "Create" button
- **Unify MCP Server:** REST API fallback: POST /v2/applications/{appId}/target-groups — used to create the target group programmatically if using the MCP-driven workflow instead of the UI form directly

**Example Prompts to trigger skill**

- "Create the target group now"
- "Save this segment as a reusable group"
- "Test my user ID against this target group"

**Constraints**

- Must give the group a meaningful, descriptive name — not a generic name like "group1"
- Must include a description documenting the group's purpose and conditions before saving
- Must confirm creation by reading the target group detail page shown after saving
- Must not create duplicate groups with the same conditions (checked during targeting-intent-parser)

**Acceptance Criteria**

The user is on the Create New Target Group page with a descriptive name and description entered. The "Save" button has been clicked and the page has transitioned to the target group detail view confirming creation. The user knows how to test it with a user ID and how to apply it to a flag rollout from the Flags page.
