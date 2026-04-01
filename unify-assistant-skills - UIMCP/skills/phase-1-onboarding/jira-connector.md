**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to connect CloudBees FM to Jira so my team can link feature flags to Jira tickets and see issue context directly on the flag page.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Integrations > Jira** page in the Unify FM UI and asks "How do I connect this to Jira?"
2. Assistant reads the form fields and options visible on the page: Jira instance URL field, authentication type selector (Cloud vs Server/Data Center), API token or credentials field, and the "Test Connection" button
3. Assistant asks whether the user is on Jira Cloud or Jira Server/Data Center (to match the authentication method shown on the page)
4. Assistant guides the user to generate a Jira API token in Jira (Account Settings > Security > API Tokens for Cloud)
5. User enters their Jira instance URL and API token into the fields visible on the current page
6. Assistant instructs the user to click the "Test Connection" button on the page to validate the credentials
7. On success, the page shows a connected status — assistant confirms the connection and shows the user how to link a flag to a Jira issue from the flag detail page

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Integrations > Jira
- **Page content to use:** The Jira instance URL input field, authentication type selector (Cloud vs Server), API token/credentials input, and the "Test Connection" button and connection status indicator on the page
- **Unify MCP Server:** None required for this step

**Example Prompts to trigger skill**

- "Help me integrate Jira with Feature Management"
- "Connect my Jira tickets to FM flags"
- "Set up the Jira integration"
- "Link flags to Jira issues"

**Constraints**

- Must handle Jira Server differently from Jira Cloud (different auth method shown on the page)
- Must instruct the user to use the Test Connection button on the page before declaring success
- Must not store Jira credentials — the user enters them directly into the page
- Must explain what data will be accessed in Jira (read-only issue data)

**Acceptance Criteria**

The user is on the Settings > Integrations > Jira page with credentials entered into the visible form fields. The Test Connection button on the page has been used and shows a successful connection status. The user knows how to link a feature flag to a Jira issue and can see issue information (title, status) on the flag detail page.
