**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

Before creating my target group, I want to estimate how many users will match my targeting rules so I can verify the group size makes sense before using it in production.

**Conversation Flow (Happy Path)**

1. User is on the **Target Groups > (target group) > Preview** section in the Unify FM UI and asks "How many users will match these rules?"
2. Assistant reads the preview data visible on the page: estimated match count, percentage of total user base shown, and any sample matching user entries displayed
3. Assistant presents the data read from the page: total estimated matches, percentage of total users, and a description of the sample users shown (without exposing personally identifiable information)
4. Assistant warns if the preview shows an unexpectedly large group (>50% of users) — asks the user if this is intentional
5. Assistant warns if the preview shows an unexpectedly small group (<10 users) or 0 users — explains this likely indicates a rule error
6. If 0 users match, assistant guides the user back to the conditions form to review the conditions entered
7. User confirms the audience size looks correct and the assistant tells them to save the target group using the "Save" or "Create" button on the page

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Target Groups > (target group) > Preview
- **Page content to use:** The Preview section — estimated match count, percentage of total users, and sample matching user entries shown on the page
- **Unify MCP Server:** None required for this step (audience estimation is read directly from the Preview section visible on the page)

**Example Prompts to trigger skill**

- "How many users will match my target group?"
- "Estimate the audience size for this segment"
- "Preview which users match before I create the group"
- "Is my target group too large?"

**Constraints**

- Must warn if the preview shows 0 users — indicates a likely rule error and must not proceed without addressing it
- Must warn if the preview shows more than 50% of total users without explicit confirmation
- Must be clear that the estimate shown on the page is approximate and based on current data
- Must not describe specific user identities visible in the preview

**Acceptance Criteria**

The user is on the Target Group Preview section. The assistant has read and presented the match count and percentage from the page. Any size warnings (too large, too small, or zero) have been surfaced and acknowledged. The user confirms the audience size looks correct and is ready to save the target group.
