**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to understand which third-party integrations (Jira, Slack, Webhooks, etc.) would be most useful for my team and where to start.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Integrations** overview page in the Unify FM UI and asks "Which integrations should I set up?"
2. Assistant reads the list of available integrations visible on the page and their current connection status (connected, not connected, available)
3. Assistant asks what collaboration tools the team currently uses (e.g., Jira, Slack, Teams, Datadog)
4. Assistant asks about key use cases: "Do you want notifications when flags change? Do you want to link flags to tickets?"
5. Assistant recommends 1–2 integrations to start with based on the team's tools and use cases — pointing to the specific integration tiles visible on the Settings > Integrations page, with estimated setup time for each
6. User selects which integration to set up first, and the assistant instructs them to click the corresponding integration tile on the current page

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Integrations (overview page)
- **Page content to use:** The list of integration tiles shown on the page — integration names (Jira, Slack, Webhooks, Code Repositories, etc.), their connection status indicators (connected / not connected), and the clickable tiles to enter each integration's setup page
- **Unify MCP Server:** None required for this step

**Example Prompts to trigger skill**

- "What integrations does CloudBees FM support?"
- "How do I connect FM to Jira?"
- "Help me figure out which integrations to set up"
- "Connect CloudBees FM to our tools"

**Constraints**

- Must not recommend integrations that require admin access the user does not have
- Must not overwhelm the user by suggesting all integrations at once — prioritize 1–2 to start
- Must explain the value of each recommended integration before suggesting setup
- Must reference the integration tiles and statuses visible on the Settings > Integrations page

**Acceptance Criteria**

The user is on the Settings > Integrations overview page. The assistant has read the available integration tiles and their statuses. The user has selected 1–2 integrations to set up based on their team's needs, understands the value of each, and is ready to click into the first integration's setup page.
