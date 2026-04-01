**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I want to generate documentation that explains my environment setup to my team so everyone knows the purpose of each environment, how to use SDK keys, and how to initialize the SDK.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Environments** list view in the Unify FM UI and asks "Can you help me document my environment setup for my team?"
2. Assistant reads the environment list visible on the page: names, types, and order of all environments shown
3. Assistant calls `flags_environments_list` to retrieve the full environment data including names, descriptions, and any additional metadata
4. Assistant uses the data from both the page and `flags_environments_list` to generate an ENVIRONMENTS.md document summarizing all environments: name, purpose, and SDK key variable name
5. Assistant adds SDK integration code examples per language/framework
6. Assistant includes troubleshooting tips for common environment issues
7. Assistant explains how to keep the documentation up to date as environments change (update when adding/removing environments via Settings > Environments)
8. User reviews the draft and the assistant adjusts based on feedback

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Environments (list view)
- **Page content to use:** The full list of environments shown on the page — environment names, types, order, and any description labels visible in the list
- **Unify MCP Server:** `flags_environments_list` (params: applicationId) — used to retrieve complete environment metadata to populate the documentation accurately

**Example Prompts to trigger skill**

- "Create documentation for my environment setup"
- "Generate ENVIRONMENTS.md for my team"
- "Document how to use SDK keys"
- "Create onboarding docs for feature flags"

**Constraints**

- Must use variable names for SDK keys, never real key values
- Must tailor code examples to the user's programming language
- Must be written for team members who are new to FM
- Must not include internal CloudBees infrastructure references
- Must base the documentation on the actual environments returned by `flags_environments_list` — not generic examples

**Acceptance Criteria**

The user is on the Settings > Environments list page. `flags_environments_list` has been called to retrieve environment data. An ENVIRONMENTS.md document is generated that documents all environments (matching what is visible on the page), their purposes, SDK key variable names, code initialization examples, and basic troubleshooting guidance. A new team member could follow this document to set up FM independently.
