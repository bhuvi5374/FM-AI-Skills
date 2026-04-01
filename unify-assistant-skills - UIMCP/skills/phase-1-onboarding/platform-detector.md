**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to connect my code repository to CloudBees Feature Management so I can track where feature flags are used in my codebase.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Integrations > Code Repositories** page in the Unify FM UI and asks "How do I connect my repo to Feature Management?"
2. Assistant reads the list of available integration options visible on the page (GitHub, and any others shown)
3. Assistant asks which platform the user's repository is on
4. If the user says GitHub, assistant confirms GitHub is supported and instructs the user to click "Add Integration" on the current page to proceed
5. If the user says any other platform (GitLab, Bitbucket, Azure DevOps, etc.), assistant explains that only GitHub is supported today and suggests they follow up with the CloudBees team for future platform support
6. Assistant confirms the user is ready to proceed and directs them to the next step (clicking "Add Integration")

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Integrations > Code Repositories
- **Page content to use:** The list of available repository integration options displayed on the page (which platforms are shown as supported), any already-connected repositories listed, and the "Add Integration" button
- **Unify MCP Server:** None required for this step

**Example Prompts to trigger skill**

- "Help me connect my GitHub repo to CloudBees FM"
- "How do I set up code repository integration?"
- "Link my codebase to Feature Management"
- "Enable code scanning for feature flags"

**Constraints**

- Must ask which platform the user is on before proceeding
- If the user is not on GitHub, must clearly state that only GitHub is supported today — must not attempt to guide setup for unsupported platforms
- Must not request access credentials at this stage (handled by auth-credential-guide)
- Must reference what the user can see on the current Settings > Integrations > Code Repositories page

**Acceptance Criteria**

The user is on the Settings > Integrations > Code Repositories page. The assistant has confirmed the user's platform. If GitHub, the user is directed to click "Add Integration" to proceed. If another platform, the user is informed only GitHub is supported today. The UI page state shows the user is positioned correctly to take the next action.
