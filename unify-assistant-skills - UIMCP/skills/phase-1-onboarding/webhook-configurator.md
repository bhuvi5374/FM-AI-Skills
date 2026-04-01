**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to set up webhooks so my own systems or automation tools receive real-time events when feature flags change in CloudBees FM.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Integrations > Webhooks** page in the Unify FM UI and asks "How do I set up a webhook?"
2. Assistant reads the page content: the webhook list (if any exist), the "Add Webhook" button, and any configuration panels visible
3. Assistant explains webhook use cases (e.g., trigger deployments, update internal dashboards, feed audit logs)
4. User clicks "Add Webhook" on the page — assistant reads the webhook configuration form that appears: endpoint URL field, event type checkboxes, authentication options
5. User provides their webhook endpoint URL and enters it into the URL field on the form
6. Assistant helps the user select which events to subscribe to using the event checkboxes on the page (e.g., flag.rollout.changed, flag.created, flag.deleted, approval.requested)
7. Assistant guides the user through configuring authentication using the options shown on the form (Bearer token or HMAC signature)
8. Assistant instructs the user to save the webhook and use the "Send Test Event" button visible on the page to confirm the endpoint responds with 200 OK

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Integrations > Webhooks
- **Page content to use:** The webhook list, "Add Webhook" button, and the webhook configuration form fields — endpoint URL input, event type checkboxes (flag.rollout.changed, flag.created, etc.), authentication type selector, authentication secret field, and the "Send Test Event" button
- **Unify MCP Server:** None required for this step

**Example Prompts to trigger skill**

- "Configure webhooks for flag events"
- "Set up a webhook to trigger my deployment pipeline"
- "Send events to my system when flags change"
- "Set up real-time FM notifications"

**Constraints**

- Must not save the webhook without the user confirming the endpoint URL in the form field
- Must explain what data is included in each event payload before the user selects event types
- Must guide the user to configure authentication — must not leave authentication blank
- Must instruct the user to use the test event button on the page before declaring setup complete

**Acceptance Criteria**

The user is on the Settings > Integrations > Webhooks page with the webhook configured using the form fields on the page. The correct events are selected, authentication is configured, and the test event button has confirmed a 200 OK response. The user understands how to view delivery logs and troubleshoot failures from the webhooks page.
