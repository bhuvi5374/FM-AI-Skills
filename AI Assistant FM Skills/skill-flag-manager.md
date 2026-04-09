# Skill: Feature Flag Manager

## Your Information

Name: Bhuvi / CloudBees FM Team
Date: 2026-04-09
Product Area: Unify Feature Management

---

## Prompts & Interactions

### User Goal/Problem being addressed

User wants to create, view, update, or delete/ a feature flag in CloudBees Unify Feature Management. This is a single skill that handles all core flag lifecycle operations — the assistant detects intent from the user's natural language and executes the appropriate action.

---

## Conversation Flow (Happy Path)

### Intent: Create a Flag

User: "Create a flag for the dark mode feature"

A: Got it! Let me set up the flag. A few quick questions:
- What should we name it? (e.g., `dark-mode-enabled`)
- What type? Boolean (on/off) or Multivariate (multiple values)?
- Any description or tags you'd like to add?

User: "dark-mode-enabled, boolean, for the UI team"

A: Here's what I'll create:
- **Name:** dark-mode-enabled
- **Type:** Boolean
- **Tags:** ui-team
- **Environments:** [lists available environments with default OFF state]

Shall I go ahead?

User: "Yes"

A: [calls flag create tool] ✓ Flag **dark-mode-enabled** created! It's now live in your environments (defaulting to OFF). You can find it under Flags → dark-mode-enabled.

---

### Intent: View / List Flags

User: "Show me all my flags" / "What flags do we have for the checkout flow?"

A: [calls flag list tool] Here are your feature flags: [returns filtered/full list with name, status, type, last modified]

A: Would you like to filter by environment, tag, or status (active/d)?

---

### Intent: Update a Flag

User: "Update the dark-mode flag — add the mobile team tag" / "Change the rollout to 50%"

A: [calls flag list tool to find the flag] Found **dark-mode-enabled**. Here's what I'll change:
- Add tag: `mobile-team`

Confirm?

User: "Yes"

A: [calls flag update tool] ✓ Flag updated. Tags now include: ui-team, mobile-team.

---

### Intent: Delete a Flag

User: "Delete the dark-mode flag" / "Clean up dark-mode-enabled"

A: Before I  — let me check if this flag is still active in any environment.

[calls flag list tool to check status and targeting rules]

A: **dark-mode-enabled** is currently:
- OFF in all environments
- No active targeting rules
- Last evaluated: 14 days ago

Safe to . Shall I proceed?

User: "Yes"

A: [calls flag  tool] ✓ Flag d. It will no longer be evaluated. You can restore it anytime from the Archived Flags section.

---

## Context Requirements (Platform APIs/MCP Tools Used)

> **Note for developers:** Fill in the verified tool names and REST API paths from the Unify MCP Server registry and the official CloudBees FM API reference:
> https://docs.cloudbees.com/docs/cloudbees-unify/latest/api-references/Feature-management/

| Action | MCP Tool / Frontend API | REST API |
|--------|------------------------|----------|
| Create flag | _[dev to fill in]_ | _[dev to fill in]_ |
| List / search flags | _[dev to fill in]_ | _[dev to fill in]_ |
| Update flag (name, description, tags) | _[dev to fill in]_ | _[dev to fill in]_ |
| Update targeting / rollout % | _[dev to fill in]_ | _[dev to fill in]_ |
| Archive flag | _[dev to fill in]_ | _[dev to fill in]_ |
| Restore d flag | _[dev to fill in]_ | _[dev to fill in]_ |

**Conversation context needed:**
- Current application/project context (appId)
- Available environments for the workspace
- Existing flag list (for duplicate detection on create, and status check on )

---

## Example Prompts to trigger skill

**Create:**
- "Create a flag for dark mode"
- "Add a new boolean flag called checkout-v2"
- "I need a feature flag for the mobile redesign"

**View / List:**
- "Show me all flags"
- "What flags are in the checkout flow?"
- "List flags tagged with mobile"
- "Is the dark-mode flag on?"

**Update:**
- "Update the dark-mode flag description"
- "Add a tag to checkout-v2"
- "Rename flag X to Y"
- "Change the rollout to 25%"

**Delete:**
- "Delete the dark-mode flag"
- "Clean up old flags"
- "Delete checkout-v2 — it's been fully rolled out"
- "The flag is dead, remove it"

---

## Constraints

- **Never create a flag without user confirmation** of name, type, and environment defaults
- **Always check for duplicate names** before creating
- **Always check flag status before archiving** — warn if flag is ON in any environment or has active targeting rules
- Do not permanently delete flags — use  only (deletion is irreversible)
- Do not update targeting rules without explicit user confirmation of the change
- Do not assume appId — always resolve from current context or ask the user

---

## Suggested Next Steps

After creating a flag:
- Set up targeting rules or rollout percentages
- Generate SDK code snippet for the flag

After Deleting:
- Suggest removing the flag reference from code (triggers Flag Cleanup skill)

---

## Acceptance Criteria

- **Create:** Flag appears in flag list with correct name, type, and default states. User is shown the flag detail page.
- **View:** User sees a filtered or full flag list with status, type, and last modified date.
- **Update:** Flag detail reflects the change. Confirmed via flag list or flag detail read.
- **Archive:** Flag no longer appears in the active flag list. User is informed they can restore it.
