**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I've tested my first feature flag locally and am ready to commit my code and deploy it to the development environment.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Configuration > Development environment** view in the Unify FM UI and asks "I'm ready to deploy — what should I check here first?"
2. Assistant reads the flag's Development environment configuration visible on the page: current value (ON/OFF), rollout percentage or targeting rules shown
3. Assistant calls `flags_configurations_list` to confirm the flag's Development environment is set to ON (as configured during flag creation)
4. Assistant provides git commit guidance for the user's code changes (git add, git commit with a clear message, git push)
5. Assistant guides the user through their normal deployment process to the development environment
6. After deployment, assistant instructs the user to return to the flag's Configuration > Development environment page and verify the flag value shown matches what was configured (ON for dev)
7. Assistant confirms the feature appears correctly in the deployed dev environment
8. Assistant congratulates the user and summarizes what they have learned, then suggests next steps: try a gradual rollout, learn about target groups, explore flag analytics — all accessible from the flag detail page

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Configuration > Development environment
- **Page content to use:** The Development environment flag value indicator (ON/OFF), rollout percentage shown, targeting rules (if any), and any deployment status indicators on the page
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to confirm the Development environment's configured value is ON before the user deploys

**Example Prompts to trigger skill**

- "I'm ready to deploy my first flag"
- "Help me commit and push my flag code"
- "Deploy my feature flag to dev"
- "What's next after local testing?"

**Constraints**

- Must not suggest deploying directly to production as a first deployment
- Must use `flags_configurations_list` to verify the Development environment is set to ON before the user deploys
- Must instruct the user to verify the flag works in the dev environment after deployment by returning to this page
- Must celebrate the user's first flag as a positive milestone and provide clear next steps

**Acceptance Criteria**

The user is on the flag's Development environment Configuration page. `flags_configurations_list` has confirmed the Development environment is set to ON. The user has committed and pushed their code and deployed to the development environment. The flag Configuration page for Development shows the correct ON state post-deployment. The user understands the flag lifecycle and knows what to explore next.
