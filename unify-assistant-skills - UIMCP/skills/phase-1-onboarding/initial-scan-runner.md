**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to run the first code scan on my connected repository to discover all existing feature flag references and validate the integration is working.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Integrations > Code Repositories > (connected repo)** page in the Unify FM UI and asks "How do I run my first scan?"
2. Assistant reads the page content: the connected repository name, last scan status (showing "Never scanned" or a prior scan date), the "Run Scan" or "Trigger Scan" button visible on the page
3. Assistant instructs the user to click the "Run Scan" button visible on the current page
4. Assistant explains what to expect: scan progress indicator will appear, duration depends on repo size
5. When the scan completes, the page updates with results — assistant reads the displayed results: total flags found, number of code references, and files listed
6. Assistant calls `flags_configurations_list` to verify that the flags discovered by the scan now appear in the FM flags list
7. Assistant confirms the integration is healthy by cross-referencing the scan results on the page with the flags returned by the MCP tool

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Integrations > Code Repositories > (connected repo)
- **Page content to use:** Repository connection status, last scan date/status indicator, "Run Scan" button, scan results panel (flags found count, code references count, files listed after scan completes)
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used after scan completes to verify that discovered flags now appear in the FM flags list

**Example Prompts to trigger skill**

- "Run a code scan on my repository"
- "Scan my codebase for feature flags"
- "Show me all flags in my code"
- "Trigger first scan"

**Constraints**

- Must not proceed to completion if 0 flags are found and user expected flags to exist (prompt to debug)
- Must instruct the user to use the scan trigger button visible on the page — not an API call
- Must validate results using `flags_configurations_list` before declaring success
- Must handle scan timeout gracefully and allow restart
- Must reference the scan status and results visible on the page

**Acceptance Criteria**

The user is on the connected repo page and has triggered the scan using the button on the page. Scan results are visible on the page (flags found, code references, file locations). `flags_configurations_list` confirms those flags appear in FM. The user can see that their existing flags are detected and understands where to view code references in the FM UI.
