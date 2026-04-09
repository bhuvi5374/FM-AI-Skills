# Skill: Flag Cleanup

## Your Information

Name: Bhuvi / CloudBees FM Team
Date: 2026-04-09
Product Area: Unify Feature Management

---

## Prompts & Interactions

### User Goal/Problem being addressed

User wants to identify and clean up stale or redundant feature flags. A flag is considered stale when it has been at 100% rollout for an extended period, has not been evaluated recently, or is no longer referenced in code. This skill helps users discover, review, and safely archive stale flags — and guides them to remove dead code references.

---

## Conversation Flow (Happy Path)

### Intent: Find Stale Flags

User: "Show me stale flags" / "Which flags should I clean up?"

A: [calls flag list tool to scan all flags] Let me check your flags for cleanup candidates. I'll look for:
- Flags at 100% rollout for more than 14 days
- Flags with no recent evaluations
- Archived flags still referenced in code

A: Here's what I found:

**Cleanup Candidates:**
| Flag | Status | Rollout | Last Evaluated | Risk |
|------|--------|---------|---------------|------|
| dark-mode-enabled | Active | 100% | 21 days ago | Low |
| old-checkout | Active | 100% | 45 days ago | Low |
| experiment-nav | Active | 0% | 90 days ago | Medium |

Which flag would you like to clean up first?

---

### Intent: Clean Up a Specific Flag

User: "Clean up dark-mode-enabled"

A: [calls flag detail tool] Here's the full picture for **dark-mode-enabled**:
- **Rollout:** 100% in Production for 21 days
- **Targeting rules:** None
- **Last evaluated:** 21 days ago
- **Environments:** Production (100%), Staging (100%)

**Recommended action:** Archive this flag. Remind your team to remove the flag reference from the codebase.

Shall I archive it?

User: "Yes"

A: [calls flag archive tool] ✓ **dark-mode-enabled** has been archived.

Next step: Search your codebase for references to `dark-mode-enabled` and remove the dead code block (the 100% ON path is now permanent — the flag check is no longer needed).

---

### Intent: View All Archived Flags

User: "Show me all archived flags" / "What flags have we already cleaned up?"

A: [calls flag list tool with archived filter] Here are your archived flags: [returns list with name, archive date, last evaluated before archive]

---

## Context Requirements (Platform APIs/MCP Tools Used)

> **Note for developers:** Fill in the verified tool names and REST API paths from the Unify MCP Server registry and the official CloudBees FM API reference:
> https://docs.cloudbees.com/docs/cloudbees-unify/latest/api-references/Feature-management/

| Action | MCP Tool / Frontend API | REST API |
|--------|------------------------|----------|
| List all active flags with rollout/eval data | _[dev to fill in]_ | _[dev to fill in]_ |
| Get flag detail (rollout %, last evaluated, rules) | _[dev to fill in]_ | _[dev to fill in]_ |
| Archive flag | _[dev to fill in]_ | _[dev to fill in]_ |
| List archived flags | _[dev to fill in]_ | _[dev to fill in]_ |

**Conversation context needed:**
- Full flag list with rollout %, last evaluation date, and targeting rules
- Environment-level flag state

---

## Example Prompts to trigger skill

**Find stale flags:**
- "Show me stale flags"
- "Which flags should I clean up?"
- "What flags have been at 100% for a while?"
- "Find flags that haven't been used recently"
- "Give me a cleanup report"

**Clean up a specific flag:**
- "Clean up dark-mode-enabled"
- "Archive old-checkout, it's been fully rolled out"
- "Remove the experiment-nav flag"

**View archived flags:**
- "Show me all archived flags"
- "What flags have we cleaned up already?"

---

## Constraints

- **Never archive a flag that is not at 100% or 0%** without explicit user confirmation and a clear warning
- **Always show the full flag status** (rollout %, last evaluated, targeting rules) before archiving
- **Always remind the user to remove code references** after archiving — the assistant cannot modify code directly
- Do not archive flags in bulk without individual confirmation for each
- Do not assume a flag is safe to archive based on age alone — always check rollout % and targeting rules

---

## Suggested Next Steps

After archiving a flag:
- Remove the flag reference and dead code block from the codebase
- Verify no SDK calls reference the archived flag key

---

## Acceptance Criteria

- **Find stale flags:** User sees a list of cleanup candidates with rollout %, last evaluated date, and risk level.
- **Clean up a flag:** Flag is archived after user confirmation. User is reminded to clean up code references.
- **View archived flags:** User sees the full list of previously archived flags.
