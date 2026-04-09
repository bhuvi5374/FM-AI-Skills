# Skill: Custom Property Manager

## Your Information

Name: Bhuvi / CloudBees FM Team
Date: 2026-04-09
Product Area: Unify Feature Management

---

## Prompts & Interactions

### User Goal/Problem being addressed

User wants to create, view, update, or delete a custom property in CloudBees Unify Feature Management. Custom properties are attributes (e.g., plan type, country, company size) used to define audiences and build targeting rules. This is a single skill that handles all core custom property lifecycle operations — the assistant detects intent from the user's natural language and executes the appropriate action.

---

## Conversation Flow (Happy Path)

### Intent: Create a Custom Property

User: "I need a custom property for subscription plan"

A: Got it! Let me set that up. A few quick questions:
- What should we name it? (e.g., `subscription-plan`)
- What type of values will it hold? String (text), Boolean (true/false), or Number?
- Any allowed values to restrict to? (e.g., `free`, `pro`, `enterprise`) or leave it open?
- Any description?

User: "subscription-plan, string, values: free, pro, enterprise"

A: Here's what I'll create:
- **Name:** subscription-plan
- **Type:** String
- **Allowed Values:** free, pro, enterprise
- **Description:** —

Shall I go ahead?

User: "Yes"

A: [calls custom property create tool] ✓ Custom property **subscription-plan** created! It's now available to use in targeting rules. You can find it under Custom Properties → subscription-plan.

---

### Intent: View / List Custom Properties

User: "Show me all custom properties" / "What properties do we have for targeting?"

A: [calls custom property list tool] Here are your custom properties: [returns list with name, type, allowed values, last modified]

A: Would you like to filter by type or search by name?

---

### Intent: Update a Custom Property

User: "Add 'enterprise-plus' as an allowed value to subscription-plan" / "Update the description for country"

A: [calls custom property list tool to find the property] Found **subscription-plan**. Here's what I'll change:
- Add allowed value: `enterprise-plus`

Confirm?

User: "Yes"

A: [calls custom property update tool] ✓ Updated. Allowed values are now: free, pro, enterprise, enterprise-plus.

---

### Intent: Delete a Custom Property

User: "Delete the subscription-plan property" / "Remove the country custom property"

A: Before I delete — let me check if **subscription-plan** is currently used in any targeting rules.

[calls custom property list tool to check usage]

A: **subscription-plan** is currently:
- Used in 2 targeting rules (dark-mode-enabled, checkout-v2)

Deleting this property will affect those targeting rules. Are you sure you want to proceed?

User: "Yes"

A: [calls custom property delete tool] ✓ Custom property deleted. Note: the targeting rules referencing this property will need to be updated manually.

---

## Context Requirements (Platform APIs/MCP Tools Used)

> **Note for developers:** Fill in the verified tool names and REST API paths from the Unify MCP Server registry and the official CloudBees FM API reference:
> https://docs.cloudbees.com/docs/cloudbees-unify/latest/api-references/Feature-management/

| Action | MCP Tool / Frontend API | REST API |
|--------|------------------------|----------|
| Create custom property | _[dev to fill in]_ | _[dev to fill in]_ |
| List / search custom properties | _[dev to fill in]_ | _[dev to fill in]_ |
| Update custom property | _[dev to fill in]_ | _[dev to fill in]_ |
| Delete custom property | _[dev to fill in]_ | _[dev to fill in]_ |

**Conversation context needed:**
- Current application/project context (appId)
- Existing custom properties list (for duplicate detection on create, and usage check on delete)
- Targeting rules that reference the property (for safe delete warning)

---

## Example Prompts to trigger skill

**Create:**
- "Create a custom property for subscription plan"
- "Add a new string property called country"
- "I need a property to track user role"

**View / List:**
- "Show me all custom properties"
- "What properties are available for targeting?"
- "List all boolean properties"

**Update:**
- "Add a new allowed value to subscription-plan"
- "Update the description for the country property"
- "Rename property X to Y"

**Delete:**
- "Delete the subscription-plan property"
- "Remove the country custom property, we don't use it anymore"

---

## Constraints

- **Never create a custom property without user confirmation** of name and type
- **Always check for duplicate names** before creating
- **Always check if a property is used in targeting rules before deleting** — warn the user with the list of affected rules
- Do not delete a property silently — always surface the impact first
- Do not assume appId — always resolve from current context or ask the user

---

## Suggested Next Steps

After creating a custom property:
- Use it in a targeting rule (triggers Flag Targeting Manager skill)
- Add it to a target group definition

After deleting:
- Review and update any targeting rules that referenced this property

---

## Acceptance Criteria

- **Create:** Custom property appears in the property list with correct name, type, and allowed values.
- **View:** User sees a full or filtered list of custom properties with type and allowed values.
- **Update:** Property reflects the change. Confirmed via property list or property detail read.
- **Delete:** Property no longer appears in the list. User was warned about impacted targeting rules before deletion.
