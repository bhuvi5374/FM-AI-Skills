**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to validate that all my integrations (Jira, Slack, webhooks) are working correctly after setup before relying on them in daily workflows.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (test flag) > Overview** page in the Unify FM UI and asks "Can you help me test my integrations are working?"
2. Assistant reads the flag name and current configuration shown on the Overview page (flag status, linked Jira issue if any, environment configuration)
3. Assistant calls `flags_configurations_list` to retrieve the test flag's full configuration and confirm which environments it exists in
4. Assistant calls `flags_environments_list` to confirm the full environment list, so the assistant knows which environments to test the rollout change in
5. Assistant instructs the user to perform a rollout change on the test flag using the controls visible on the Overview page (e.g., toggle the flag ON in an environment)
6. Assistant checks each integration result: Jira link visible on the flag Overview page, Slack notification received (user confirms), webhook delivery (user checks endpoint logs)
7. Assistant reports pass/fail for each integration based on what the user reports from the page and external tools
8. On failure, assistant provides specific error details and next steps (e.g., return to Settings > Integrations > [integration] to reconfigure)
9. After validation, assistant instructs the user to reset the test flag to its original state using the controls on the Overview page

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (test flag) > Overview
- **Page content to use:** The test flag's name, status indicators, Jira link section, environment rollout controls, and any integration status indicators visible on the Overview page
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — to retrieve the test flag's full configuration; `flags_environments_list` (params: applicationId) — to confirm available environments for rollout testing

**Example Prompts to trigger skill**

- "Test my integrations"
- "Verify Jira and Slack are working"
- "Run integration validation"
- "Check that my webhook is receiving events"

**Constraints**

- Must use a clearly named test flag — must not make changes to real production flags during testing
- Must test each configured integration, not just one
- Must provide specific failure details if an integration does not pass
- Must instruct the user to reset the test flag after validation using the controls on the Overview page

**Acceptance Criteria**

The user is on a test flag's Overview page. `flags_configurations_list` and `flags_environments_list` have been called to confirm the flag and environments. A rollout change was made using the page controls. All configured integrations have been validated (Jira link visible on page, Slack notification confirmed, webhook event received). Any failures are reported with clear next steps. The test flag is reset to its original state.
