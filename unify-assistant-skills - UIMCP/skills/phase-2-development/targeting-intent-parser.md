**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to target my feature flag to a specific group of users but need help translating my business criteria (e.g., "beta users with premium accounts") into targeting rules CloudBees FM can use.

**Conversation Flow (Happy Path)**

1. User is on the **Target Groups** main list page in the Unify FM UI and asks "I need to create a target group for beta users" or "How do I target only premium subscribers?"
2. Assistant reads the target groups list visible on the page: existing group names, any descriptions shown, and the "Create Target Group" button
3. Assistant asks clarifying questions to understand the targeting criteria in plain language: "What makes someone a beta user? Which user attribute identifies that?"
4. Assistant breaks down compound criteria (e.g., "beta users AND premium accounts" → two conditions with AND logic)
5. Assistant identifies whether simple or complex targeting is needed based on the user's description
6. Assistant checks whether any existing target groups shown on the page could already cover the stated criteria (to avoid duplicate groups)
7. Assistant confirms the targeting intent with the user and instructs them to click "Create Target Group" on the current page to proceed

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Target Groups (main list page)
- **Page content to use:** The list of existing target groups shown on the page — group names, descriptions, and the "Create Target Group" button
- **Unify MCP Server:** None required for this step (custom properties are not surfaced via current MCP tools; the assistant reads the page and asks the user)

**Example Prompts to trigger skill**

- "Help me create a target group for beta users"
- "I want to target only premium subscribers"
- "Show this feature to users in the US only"
- "Create a segment for internal testing"

**Constraints**

- Must not make assumptions about available user properties — confirm they exist before building rules
- Must ask clarifying questions before building rules
- Must flag if the stated criteria cannot be expressed with available properties
- Must check existing target groups on the page to avoid recommending duplicate groups

**Acceptance Criteria**

The user is on the Target Groups list page. The assistant has read the existing groups on the page and checked for duplicates. The assistant has correctly understood the user's targeting intent, broken it down into specific conditions, confirmed the targeting logic (AND/OR), and the user is ready to click "Create Target Group" to proceed.
