**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to understand how to use the code references feature in CloudBees FM to track where flags are used in my codebase and plan flag cleanup.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Code References tab** in the Unify FM UI and asks "What does this Code References tab show me?"
2. Assistant reads the content visible on the Code References tab: the list of file paths, line numbers, branch names, and code snippet previews shown for the current flag
3. Assistant explains what each reference shows: file name, line number, branch, and the code snippet visible on the page
4. Assistant calls `flags_configurations_list` to retrieve the full list of flags, so the assistant can help the user understand how code references fit into the broader flag inventory
5. Assistant explains how references stay up to date (automatic scan on push, based on the scan frequency configured in Settings)
6. Assistant shows the user how to use the references visible on this page for cleanup planning (e.g., the number of files shown helps estimate cleanup effort)
7. Assistant points out any "Last Scanned" date indicator on the page and explains how to trigger a manual scan if the data looks stale

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Code References tab
- **Page content to use:** The list of code references shown on the tab — file paths, line numbers, branch names, code snippets, and the "Last Scanned" date indicator
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to provide context on the full flag inventory and help the user understand this flag's references relative to others

**Example Prompts to trigger skill**

- "How do I see where a flag is used in my code?"
- "Show me the code references for this flag"
- "How do I use code references for cleanup?"
- "Where do I find flag usage in the UI?"

**Constraints**

- Must not suggest code references are real-time — explain scan frequency based on what is configured
- Must reference the actual file paths and line numbers visible on the Code References tab
- Must explain how to manually trigger a scan if data looks stale
- Must clarify that references are read-only and do not modify code

**Acceptance Criteria**

The user is on the Code References tab for a flag and understands what each reference entry shows. The assistant has used `flags_configurations_list` to provide broader context. The user knows how often data is refreshed, how to trigger a manual scan, and how to use references to support flag cleanup planning.
