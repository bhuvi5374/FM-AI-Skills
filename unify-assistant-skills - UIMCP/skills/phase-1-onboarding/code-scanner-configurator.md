**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to configure code scanning so CloudBees FM can automatically detect feature flag usage in my codebase across the right branches and languages.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Integrations > Code Repositories > (connected repo) > Scanning Configuration** page in the Unify FM UI and asks "How do I configure what to scan?"
2. Assistant reads the scanning configuration options visible on the page: language/SDK detection settings, branch selection list, scan frequency options
3. Assistant reviews the detected or selectable programming languages shown on the page and confirms which flag detection patterns apply (e.g., isFeatureEnabled('flag_name') for JavaScript)
4. User selects which branches to scan from the branch list shown on the page (e.g., main, develop)
5. User sets scan frequency using the options visible on the page: on every push, daily, or manual
6. Assistant calls `flags_environments_list` to confirm which environments are configured, so the assistant can recommend aligning scan branches to those environments
7. Assistant confirms the scanning configuration is complete and instructs the user to save and trigger the initial scan

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Integrations > Code Repositories > (connected repo) > Scanning Configuration
- **Page content to use:** Language/SDK detection settings shown on the page, branch list available for selection, scan frequency options (on push / daily / manual), and the save/trigger scan button
- **Unify MCP Server:** `flags_environments_list` (params: applicationId) — used to cross-reference configured environments with the branches the user is about to select for scanning

**Example Prompts to trigger skill**

- "Configure code scanning for my repo"
- "Set up flag detection for JavaScript"
- "Which branches should I scan?"
- "Enable code scanning for feature flags"

**Constraints**

- Must not scan branches the user has not explicitly selected from the branch list on the page
- Must not use overly generic patterns that cause false positives
- Must allow custom regex patterns if the user's codebase uses non-standard naming
- Must warn if repository is very large (>100k files) about scan time
- Must reference the actual branch list and options visible on the Scanning Configuration page

**Acceptance Criteria**

The user is on the Scanning Configuration page with the correct language patterns, selected branches, and scan frequency configured and saved. The environments returned by `flags_environments_list` align with the selected branches. The user understands what will be detected and how often scans will run.
