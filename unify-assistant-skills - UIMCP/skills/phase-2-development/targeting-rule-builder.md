**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I need to build the actual targeting conditions in CloudBees FM using the right properties, operators, and boolean logic so my feature flag reaches exactly the right users.

**Conversation Flow (Happy Path)**

1. User is on the **Target Groups > Create New Target Group** conditions form in the Unify FM UI and asks "Help me set up the targeting conditions"
2. Assistant reads the conditions form visible on the page: property selector dropdowns, operator selector (equals, contains, is one of, etc.), value input fields, and the AND/OR logic toggle between conditions
3. Assistant builds the condition structure based on the targeting intent confirmed in targeting-intent-parser (e.g., beta_user = true AND subscription_tier = "premium")
4. Assistant instructs the user to select the correct property from the dropdown shown on the page, choose the operator, and enter the value for each condition — one condition at a time
5. Assistant explains AND vs OR logic with a simple analogy as the user sets up the logic between conditions
6. Assistant shows a preview of the rule by describing which users would match vs not match based on the conditions entered, with concrete examples
7. User reviews the conditions shown on the form and confirms the rule structure is correct before saving

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Target Groups > Create New Target Group (conditions form)
- **Page content to use:** The conditions form fields — property selector dropdown, operator selector, value input, AND/OR logic toggle between conditions, and the list of conditions already added to the form
- **Unify MCP Server:** None required for this step (targeting rule builder reads the form fields directly; custom properties come from the Properties tab visited in the prior step)

**Example Prompts to trigger skill**

- "Build the targeting rules for beta premium users"
- "Set up AND conditions for my target group"
- "Help me write the targeting logic"
- "Explain AND vs OR targeting"

**Constraints**

- Must instruct the user to use the property dropdowns and operator selectors visible on the form — not abstract guidance
- Must explain AND vs OR logic before applying it — do not assume the user understands
- Must validate that conditions are not logically contradictory (e.g., tier = "free" AND tier = "premium")
- Must get user confirmation that the conditions shown on the form are correct before proceeding

**Acceptance Criteria**

The user is on the Create New Target Group conditions form. The conditions have been entered into the form fields using the property dropdowns, operator selectors, and value inputs shown on the page. The user sees concrete examples of which users match vs do not match. The rule is confirmed as correct before proceeding to the audience estimator or saving.
