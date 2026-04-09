# Skill: Flag Targeting Manager

## Your Information

Name: Bhuvi / CloudBees FM Team
Date: 2026-04-09
Product Area: Unify Feature Management

---

## Prompts & Interactions

### User Goal/Problem being addressed

User wants to set up, view, update, or remove targeting rules on a feature flag. Targeting rules control which users or groups see a flag as ON or OFF based on custom properties and target groups. This single skill handles all targeting rule operations — the assistant detects intent from natural language and executes the appropriate action.

---

## Conversation Flow (Happy Path)

### Intent: Add a Targeting Rule

User: "Turn on dark-mode flag for beta users only"

A: [calls flag list tool to find the flag] Found **dark-mode-enabled**. Let me set up the targeting rule.

- **Flag:** dark-mode-enabled
- **Environment:** Which environment? (Production / Staging / All)
- **Rule:** Serve ON to target group: beta-users

User: "Production only"

A: Here's the rule I'll add:
- **Flag:** dark-mode-enabled (Production)
- **Rule:** IF user is in **beta-users** → serve ON
- **Default:** all others → OFF

Confirm?

User: "Yes"

A: [calls targeting rule create tool] ✓ Targeting rule added. dark-mode-enabled is now ON for beta-users in Production.

---

### Intent: View Targeting Rules

User: "Show me the targeting rules for dark-mode" / "Who can see the checkout-v2 flag?"

A: [calls flag targeting tool] Here are the targeting rules for **dark-mode-enabled**:

- **Production:** ON for beta-users → default OFF
- **Staging:** ON for all users

Would you like to modify any of these?

---

### Intent: Update a Targeting Rule

User: "Also enable dark-mode for enterprise-plus users in production"

A: [calls flag targeting tool to fetch current rules] Current production rule: ON for beta-users. Here's what I'll add:
- Add: IF user is in **enterprise-plus** → serve ON

Confirm?

User: "Yes"

A: [calls targeting rule update tool] ✓ Updated. dark-mode-enabled is now ON for: beta-users AND enterprise-plus users in Production.

---

### Intent: Remove a Targeting Rule

User: "Remove the beta-users rule from dark-mode in production"

A: I'll remove the targeting rule for **beta-users** on **dark-mode-enabled** in Production. After removal, those users will fall through to the default rule (OFF).

Confirm?

User: "Yes"

A: [calls targeting rule delete tool] ✓ Rule removed. beta-users in Production will now see dark-mode-enabled as OFF (default).

---

## Context Requirements (Platform APIs/MCP Tools Used)

> **Note for developers:** Fill in the verified tool names and REST API paths from the Unify MCP Server registry and the official CloudBees FM API reference:
> https://docs.cloudbees.com/docs/cloudbees-unify/latest/api-references/Feature-management/

| Action | MCP Tool / Frontend API | REST API |
|--------|------------------------|----------|
| View targeting rules for a flag | _[dev to fill in]_ | _[dev to fill in]_ |
| Add targeting rule | _[dev to fill in]_ | _[dev to fill in]_ |
| Update targeting rule | _[dev to fill in]_ | _[dev to fill in]_ |
| Remove targeting rule | _[dev to fill in]_ | _[dev to fill in]_ |

**Conversation context needed:**
- Flag name and environment context
- Available target groups and custom properties
- Current targeting rules on the flag (before any update or delete)

---

## Example Prompts to trigger skill

**Add a rule:**
- "Turn on dark-mode for beta users only"
- "Enable checkout-v2 for enterprise customers in production"
- "Show the new dashboard to users in the US"

**View rules:**
- "Show me the targeting rules for dark-mode"
- "Who can see the checkout-v2 flag?"
- "What are the current rules on new-dashboard?"

**Update rules:**
- "Also enable dark-mode for enterprise-plus users"
- "Change the rule to include UK users as well"

**Remove rules:**
- "Remove the beta-users rule from dark-mode"
- "Clear all targeting rules on checkout-v2 in staging"

---

## Constraints

- **Always confirm the environment** before adding or changing a targeting rule — production changes are irreversible without re-adding the rule
- **Never remove all rules without warning** — inform the user the flag will fall back to its default state
- **Always show the current rules before updating** so the user knows what already exists
- Do not assume which environment — always ask if unclear
- Do not assume appId — always resolve from current context or ask the user

---

## Suggested Next Steps

After adding a targeting rule:
- Verify the rollout percentage if serving to a partial audience (triggers Rollout Manager skill)
- Monitor flag evaluation results

After removing a rule:
- Confirm the default flag state is what the user expects

---

## Acceptance Criteria

- **Add:** Targeting rule appears on the flag for the correct environment. Users in the specified group receive the expected flag variation.
- **View:** User sees all current targeting rules for the flag across environments.
- **Update:** Rule reflects the change. User is shown the updated rule set.
- **Remove:** Rule no longer appears on the flag. User is informed of the fallback default behavior.
