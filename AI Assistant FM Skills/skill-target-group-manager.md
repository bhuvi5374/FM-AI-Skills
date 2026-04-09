# Skill: Target Group Manager

## Your Information

Name: Bhuvi / CloudBees FM Team
Date: 2026-04-09
Product Area: Unify Feature Management

---

## Prompts & Interactions

### User Goal/Problem being addressed

User wants to create, view, update, or delete a target group in CloudBees Unify Feature Management. Target groups are reusable audience segments (e.g., "beta users", "enterprise customers") built from custom properties and used in flag targeting rules. This single skill handles all core target group lifecycle operations — the assistant detects intent from natural language and executes the appropriate action.

---

## Conversation Flow (Happy Path)

### Intent: Create a Target Group

User: "Create a target group for beta users"

A: Let's set it up. A few questions:
- What should we name it? (e.g., `beta-users`)
- What conditions define this group? (e.g., subscription-plan = pro OR enterprise, country = US)
- Any description?

User: "beta-users, subscription-plan is pro or enterprise"

A: Here's what I'll create:
- **Name:** beta-users
- **Conditions:** subscription-plan = pro OR enterprise
- **Description:** —

Shall I go ahead?

User: "Yes"

A: [calls target group create tool] ✓ Target group **beta-users** created! It's now available to use in flag targeting rules.

---

### Intent: View / List Target Groups

User: "Show me all target groups" / "What groups do we have?"

A: [calls target group list tool] Here are your target groups: [returns list with name, conditions, number of flags using it, last modified]

A: Would you like to filter by name or see which flags are using a specific group?

---

### Intent: Update a Target Group

User: "Add enterprise-plus to the beta-users group" / "Update the conditions for the EU users group"

A: [calls target group list tool to find the group] Found **beta-users**. Current conditions: subscription-plan = pro OR enterprise. Here's what I'll change:
- Add condition: subscription-plan = enterprise-plus

Confirm?

User: "Yes"

A: [calls target group update tool] ✓ Updated. beta-users now includes: pro, enterprise, enterprise-plus.

---

### Intent: Delete a Target Group

User: "Delete the beta-users group" / "Remove the EU users target group"

A: Before I delete — let me check if **beta-users** is used in any flag targeting rules.

[calls target group list tool to check usage]

A: **beta-users** is currently referenced in 3 flags: dark-mode-enabled, checkout-v2, new-dashboard.

Deleting this group will remove it from those targeting rules. Are you sure?

User: "Yes"

A: [calls target group delete tool] ✓ Target group deleted. Note: the 3 flags that referenced this group will need their targeting rules reviewed.

---

## Context Requirements (Platform APIs/MCP Tools Used)

> **Note for developers:** Fill in the verified tool names and REST API paths from the Unify MCP Server registry and the official CloudBees FM API reference:
> https://docs.cloudbees.com/docs/cloudbees-unify/latest/api-references/Feature-management/

| Action | MCP Tool / Frontend API | REST API |
|--------|------------------------|----------|
| Create target group | _[dev to fill in]_ | _[dev to fill in]_ |
| List / search target groups | _[dev to fill in]_ | _[dev to fill in]_ |
| Update target group | _[dev to fill in]_ | _[dev to fill in]_ |
| Delete target group | _[dev to fill in]_ | _[dev to fill in]_ |

**Conversation context needed:**
- Current application/project context (appId)
- Available custom properties (to build conditions)
- Existing target groups (for duplicate detection on create, and usage check on delete)
- Flags referencing the group (for safe delete warning)

---

## Example Prompts to trigger skill

**Create:**
- "Create a target group for beta users"
- "Add a new group for enterprise customers in the US"
- "I need an audience segment for mobile users"

**View / List:**
- "Show me all target groups"
- "What groups are available for targeting?"
- "Which flags use the beta-users group?"

**Update:**
- "Add a new condition to the beta-users group"
- "Update the EU users group to include UK"
- "Rename group X to Y"

**Delete:**
- "Delete the beta-users group"
- "Remove the old mobile-testers group"

---

## Constraints

- **Never create a target group without user confirmation** of name and conditions
- **Always check for duplicate names** before creating
- **Always check if a group is used in flag targeting rules before deleting** — warn with the list of affected flags
- Do not delete a group silently — always surface the impact first
- Do not assume appId — always resolve from current context or ask the user

---

## Suggested Next Steps

After creating a target group:
- Assign it to a flag targeting rule (triggers Flag Targeting Manager skill)

After deleting:
- Review and update targeting rules on affected flags

---

## Acceptance Criteria

- **Create:** Target group appears in the group list with correct name and conditions.
- **View:** User sees a full or filtered list of target groups with conditions and flag usage count.
- **Update:** Group reflects the change. Confirmed via group list or group detail read.
- **Delete:** Group no longer appears in the list. User was warned about impacted flags before deletion.
