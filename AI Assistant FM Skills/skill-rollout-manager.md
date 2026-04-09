# Skill: Rollout Manager

## Your Information

Name: Bhuvi / CloudBees FM Team
Date: 2026-04-09
Product Area: Unify Feature Management

---

## Prompts & Interactions

### User Goal/Problem being addressed

User wants to manage the gradual rollout of a feature flag — setting rollout percentages, increasing or decreasing exposure, doing a full release, or performing an emergency kill switch. This single skill handles all rollout operations — the assistant detects intent from natural language and executes the appropriate action.

---

## Conversation Flow (Happy Path)

### Intent: Set / Update Rollout Percentage

User: "Roll out dark-mode to 20% of users"

A: [calls flag list tool to find the flag] Found **dark-mode-enabled**. Here's what I'll set:
- **Flag:** dark-mode-enabled
- **Environment:** Which environment? (Production / Staging)
- **Rollout:** 20% of users → ON, 80% → OFF

User: "Production"

A: Setting **dark-mode-enabled** to 20% rollout in Production. Confirm?

User: "Yes"

A: [calls rollout update tool] ✓ Rollout set. 20% of users in Production will now see dark-mode-enabled as ON.

---

### Intent: Increase / Decrease Rollout

User: "Increase dark-mode rollout to 50%" / "Pull back dark-mode to 10%"

A: [calls rollout tool to check current %] Current rollout for **dark-mode-enabled** in Production: 20%. 

I'll update this to **50%**. Confirm?

User: "Yes"

A: [calls rollout update tool] ✓ Rollout updated to 50%.

---

### Intent: Full Release (100%)

User: "Release dark-mode to everyone" / "Go 100% on dark-mode"

A: I'll set **dark-mode-enabled** to 100% in Production — this means all users will see it as ON. Confirm?

User: "Yes"

A: [calls rollout update tool] ✓ dark-mode-enabled is now fully released to 100% of users in Production. Consider archiving the flag once the code is cleaned up.

---

### Intent: Kill Switch (Turn Off)

User: "Kill dark-mode now" / "Turn off dark-mode for everyone immediately"

A: I'll set **dark-mode-enabled** to OFF for all users in Production (0% rollout). This is immediate. Confirm?

User: "Yes"

A: [calls rollout update tool] ✓ dark-mode-enabled is now OFF for all users in Production.

---

### Intent: View Current Rollout Status

User: "What's the rollout status of dark-mode?" / "How many users are seeing checkout-v2?"

A: [calls rollout tool] Here's the current rollout for **dark-mode-enabled**:
- **Production:** 50% ON
- **Staging:** 100% ON

---

## Context Requirements (Platform APIs/MCP Tools Used)

> **Note for developers:** Fill in the verified tool names and REST API paths from the Unify MCP Server registry and the official CloudBees FM API reference:
> https://docs.cloudbees.com/docs/cloudbees-unify/latest/api-references/Feature-management/

| Action | MCP Tool / Frontend API | REST API |
|--------|------------------------|----------|
| View current rollout % | _[dev to fill in]_ | _[dev to fill in]_ |
| Set / update rollout % | _[dev to fill in]_ | _[dev to fill in]_ |
| Full release (100%) | _[dev to fill in]_ | _[dev to fill in]_ |
| Kill switch (0% / OFF) | _[dev to fill in]_ | _[dev to fill in]_ |

**Conversation context needed:**
- Flag name and current rollout state per environment
- Environment context (production vs staging)

---

## Example Prompts to trigger skill

**Set rollout:**
- "Roll out dark-mode to 20% of users"
- "Start a 10% rollout for checkout-v2 in production"

**Increase / decrease:**
- "Increase dark-mode rollout to 50%"
- "Pull back checkout-v2 to 5%"

**Full release:**
- "Release dark-mode to everyone"
- "Go 100% on checkout-v2 in production"
- "Fully roll out new-dashboard"

**Kill switch:**
- "Kill dark-mode now"
- "Turn off checkout-v2 for everyone immediately"
- "Emergency: disable new-dashboard"

**View status:**
- "What's the rollout status of dark-mode?"
- "How many users are seeing checkout-v2?"
- "Show me rollout progress across all flags"

---

## Constraints

- **Always confirm the environment** before changing any rollout percentage — especially for production
- **Always confirm before kill switch** — turning a flag OFF for all users is high impact
- **Always confirm before full release (100%)** — and remind the user to plan flag cleanup
- Do not change rollout percentage without showing the current % first
- Do not assume appId or environment — always resolve from context or ask

---

## Suggested Next Steps

After reaching 100% rollout:
- Plan flag cleanup to remove dead code (triggers Flag Cleanup skill)

After a kill switch:
- Investigate the issue before re-enabling
- Consider reducing rollout % instead of a full kill if partial exposure is acceptable

---

## Acceptance Criteria

- **Set rollout:** Flag shows the correct rollout % for the specified environment.
- **Increase/decrease:** Rollout % reflects the updated value. User is shown before and after.
- **Full release:** Flag is at 100% in the specified environment. User is reminded to plan cleanup.
- **Kill switch:** Flag is OFF (0%) for all users in the specified environment. Confirmed immediately.
- **View:** User sees rollout % per environment for the specified flag.
