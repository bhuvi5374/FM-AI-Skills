**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want my team to be automatically notified when a new feature flag is created so everyone is aware and can use it without manual announcements.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (newly created flag) > Overview** page in the Unify FM UI and asks "How do I let my team know this flag is ready?"
2. Assistant reads the flag detail information visible on the Overview page: flag name, description, Jira link (if shown), flag creator, creation date, and the direct page URL
3. Assistant calls `flags_configurations_list` to retrieve the flag's full details to populate the notification message accurately
4. Assistant checks whether Slack integration is configured (the user can confirm, or if Slack is shown as a connected integration)
5. If Slack is configured, assistant drafts a notification message using the flag details from the Overview page and `flags_configurations_list`, including: flag name, description, Jira link, who created it, and the direct FM page URL
6. Assistant confirms the notification was sent (user confirms receipt in Slack channel)
7. If Slack is not configured, assistant generates a ready-to-send draft announcement message the user can copy and share manually via any channel

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (newly created flag) > Overview
- **Page content to use:** The flag name, description, Jira link, creator name, creation date, and page URL shown on the flag Overview page
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the full flag details to populate the notification message accurately

**Example Prompts to trigger skill**

- "Notify my team about this new flag"
- "Post to Slack about the new flag"
- "Send a flag announcement to #feature-flags"
- "Alert the team that a new flag is live"

**Constraints**

- Must not post to Slack channels that have not been configured for FM notifications
- Must not send redundant notifications if Slack already auto-notifies on flag creation
- If Slack is not configured, must provide a draft message the user can send manually
- Must not include sensitive configuration details (like SDK keys) in the notification
- Must use the flag details from the Overview page and `flags_configurations_list` — not invented content

**Acceptance Criteria**

The user is on the newly created flag's Overview page. `flags_configurations_list` has been called to retrieve the flag details. The team has been notified via Slack (or the user has a ready-to-send draft announcement). The notification includes the flag name, purpose, Jira link, and a direct link to the flag Overview page.
