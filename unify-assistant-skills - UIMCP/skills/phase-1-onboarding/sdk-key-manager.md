**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Onboarding

## Prompts & Interactions

**User Goal/Problem being addressed**

I need to securely store and distribute my CloudBees FM SDK keys to my team and integrate them into my CI/CD pipeline without accidentally committing them to version control.

**Conversation Flow (Happy Path)**

1. User is on the **Settings > Environments > (environment) > SDK Keys** page in the Unify FM UI and asks "How do I use this SDK key safely?"
2. Assistant reads the page content: the environment name, the SDK key displayed (masked), and any copy/reveal/regenerate options shown
3. Assistant calls `flags_environments_list` to confirm the full list of environments the user needs to manage keys for
4. Assistant explains the golden rule: never commit SDK keys to git
5. Assistant guides the user through storing each environment's key in environment variables (e.g., .env.local for local dev, secret manager for CI/CD) — referencing each environment returned by `flags_environments_list`
6. Assistant provides code examples for SDK initialization in the user's language (e.g., Node.js: Rox.setup(process.env.CLOUDBEES_SDK_KEY))
7. Assistant explains how to configure secrets in CI/CD tools (GitHub Actions secrets, Jenkins credentials)
8. Assistant creates a .env.example template with placeholder variable names (not real keys) and reminds the user to set up key rotation reminders

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Settings > Environments > (environment) > SDK Keys
- **Page content to use:** The environment name shown on the page, the masked SDK key display, copy/reveal options, and any regenerate key button shown
- **Unify MCP Server:** `flags_environments_list` (params: applicationId) — used to get the full list of environments so the user can manage SDK keys for all of them

**Example Prompts to trigger skill**

- "How do I store SDK keys securely?"
- "Set up environment variables for FM keys"
- "How do I add SDK keys to GitHub Actions?"
- "Create .env template for my team"

**Constraints**

- Must never output real SDK keys in documentation or examples
- Must always warn against committing keys to git
- Must provide .env.example with placeholders, not real values
- Must include key rotation guidance
- Must cover all environments returned by `flags_environments_list`, not just the current one

**Acceptance Criteria**

The user is on the SDK Keys page and understands how to safely copy and use the key shown. `flags_environments_list` has confirmed all environments needing keys. The user has a .env.example template with placeholders, understands how to configure keys in their CI/CD pipeline, and knows how to rotate keys if one is accidentally exposed.
