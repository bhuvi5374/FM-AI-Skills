**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to set up Slack notifications so my team gets alerted when feature flags change, especially in production, without having to check the FM dashboard manually.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Integrations > Slack** page in the Unify FM UI and asks "How do I connect this to Slack?"
2. Assistant reads the page content: the "Connect to Slack" or "Install Slack App" button, any OAuth authorization status indicator, and the notification channel configuration section (if visible before authorization)
3. Assistant explains what Slack permissions will be requested before the user clicks the OAuth button
4. User clicks the "Connect to Slack" button on the page, which opens the Slack OAuth flow
5. After OAuth completes, the page updates with a connected status — assistant reads the channel configuration section now visible
6. Assistant helps the user select which Slack channels should receive notifications (e.g., #feature-flags for all changes, #production-alerts for prod only) using the channel fields on the page
7. Assistant helps configure notification filters using the options visible on the page: production = always notify, staging = only rollouts >50%, dev = no notifications
8. Assistant instructs the user to use the "Send Test Notification" button on the page to verify delivery

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Integrations > Slack
- **Page content to use:** The OAuth/Connect button, connection status indicator, channel selection fields, notification filter options (by environment and event type), and the "Send Test Notification" button
- **Unify MCP Server:** None required for this step

**Example Prompts to trigger skill**

- "Set up Slack notifications for flag changes"
- "Connect FM to Slack"
- "Alert my team when flags change in production"
- "Configure Slack for CloudBees FM"

**Constraints**

- Must not send notifications to channels without user confirmation via the channel fields on the page
- Must explain what Slack permissions are required before the user clicks the OAuth button
- Must guide the user toward notification filters to avoid alert fatigue
- Must instruct the user to use the test notification button on the page before declaring setup complete

**Acceptance Criteria**

The user is on the Settings > Integrations > Slack page showing a connected status. Channel configuration and notification filters are set using the options on the page. The test notification button has been used and delivery confirmed. The user understands how to modify notification settings by returning to this page.
