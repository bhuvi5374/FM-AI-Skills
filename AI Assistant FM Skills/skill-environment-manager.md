# Skill: Environment Manager

## Your Information

Name: Bhuvi / CloudBees FM Team
Date: 2026-04-09
Product Area: Unify Feature Management

---

## Prompts & Interactions

### User Goal/Problem being addressed

User wants to create, view, update, or delete environments in CloudBees Unify Feature Management. Environments (e.g., Production, Staging, Development) define the contexts in which flags are evaluated. This single skill handles all environment lifecycle operations — the assistant detects intent from natural language and executes the appropriate action.

---

## Conversation Flow (Happy Path)

### Intent: Create an Environment

User: "Create a staging environment" / "Add a QA environment"

A: Got it! A few quick questions:
- What should we name it? (e.g., `staging`)
- Any description? (e.g., "Pre-production testing environment")
- Should new flags default to ON or OFF in this environment?

User: "staging, pre-production testing, default OFF"

A: Here's what I'll create:
- **Name:** staging
- **Description:** Pre-production testing environment
- **Default flag state:** OFF

Shall I go ahead?

User: "Yes"

A: [calls environment create tool] ✓ Environment **staging** created! All new flags will default to OFF in this environment.

---

### Intent: View / List Environments

User: "Show me all environments" / "What environments do we have?"

A: [calls environment list tool] Here are your environments:

| Name | Description | Default Flag State | Created |
|------|-------------|-------------------|---------|
| production | Live user traffic | OFF | 2024-01-10 |
| staging | Pre-production testing | OFF | 2024-01-10 |
| development | Local dev and testing | ON | 2024-03-05 |

---

### Intent: Update an Environment

User: "Update the staging environment description" / "Rename dev to development"

A: [calls environment list tool to find it] Found **staging**. Here's what I'll change:
- Update description to: "Pre-production and QA testing environment"

Confirm?

User: "Yes"

A: [calls environment update tool] ✓ Environment updated.

---

### Intent: Delete an Environment

User: "Delete the QA environment" / "Remove the old dev environment"

A: Before I delete — **QA** currently has flag configurations for 12 flags. Deleting this environment will remove all those configurations permanently.

Are you sure you want to proceed?

User: "Yes"

A: [calls environment delete tool] ✓ Environment **QA** deleted. All flag configurations for this environment have been removed.

---

## Context Requirements (Platform APIs/MCP Tools Used)

> **Note for developers:** Fill in the verified tool names and REST API paths from the Unify MCP Server registry and the official CloudBees FM API reference:
> https://docs.cloudbees.com/docs/cloudbees-unify/latest/api-references/Feature-management/

| Action | MCP Tool / Frontend API | REST API |
|--------|------------------------|----------|
| Create environment | _[dev to fill in]_ | _[dev to fill in]_ |
| List environments | _[dev to fill in]_ | _[dev to fill in]_ |
| Update environment | _[dev to fill in]_ | _[dev to fill in]_ |
| Delete environment | _[dev to fill in]_ | _[dev to fill in]_ |

**Conversation context needed:**
- Current list of environments (for duplicate detection on create, usage check on delete)
- Number of flags configured in the environment (for safe delete warning)

---

## Example Prompts to trigger skill

**Create:**
- "Create a staging environment"
- "Add a QA environment for testing"
- "I need a new environment for the mobile team"

**View / List:**
- "Show me all environments"
- "What environments do we have set up?"
- "List our environments"

**Update:**
- "Update the staging environment description"
- "Rename the dev environment to development"

**Delete:**
- "Delete the QA environment"
- "Remove the old staging environment, we don't use it anymore"

---

## Constraints

- **Never create an environment without user confirmation** of name and default flag state
- **Always check for duplicate names** before creating
- **Always warn before deleting** — show how many flag configurations will be lost
- Do not delete an environment that appears to be the primary production environment without a strong explicit confirmation
- Do not assume appId — always resolve from current context or ask the user

---

## Suggested Next Steps

After creating an environment:
- Review flag default states for the new environment
- Set up targeting rules specific to this environment (triggers Flag Targeting Manager skill)

After deleting:
- Verify SDK keys referencing this environment are also decommissioned

---

## Acceptance Criteria

- **Create:** Environment appears in the environment list with correct name and default flag state.
- **View:** User sees all environments with name, description, and default state.
- **Update:** Environment reflects the change. Confirmed via environment list.
- **Delete:** Environment no longer appears in the list. User was warned about lost flag configurations before deletion.
