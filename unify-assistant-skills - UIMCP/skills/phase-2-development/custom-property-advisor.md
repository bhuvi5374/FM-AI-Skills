**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I need to know what custom properties are available in my CloudBees FM account so I can build accurate targeting rules for my feature flag.

**Conversation Flow (Happy Path)**

1. User is on the **Target Groups > Properties tab** in the Unify FM UI and asks "What properties can I use for targeting?"
2. Assistant reads the list of custom properties visible on the Properties tab: property names, types (string, number, boolean, date, semver), and any example values or descriptions shown
3. Assistant displays the full list of properties read from the page and recommends which are most relevant for the user's targeting criteria (identified in targeting-intent-parser)
4. If a required property does not exist in the list shown on the page, assistant guides the user to create it: click "Add Property" on the current Properties tab, fill in the name and type, and set up the SDK to send that property value
5. Assistant explains what each property type means if the user is unfamiliar
6. User confirms which properties to use and the assistant tells them to proceed to the Create Target Group form

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Target Groups > Properties tab
- **Page content to use:** The list of custom properties displayed on the Properties tab — property names, data types (string, number, boolean, date, semver), example values or descriptions, and the "Add Property" button
- **Unify MCP Server:** None required for this step (custom properties are not in the current MCP tools; the assistant reads the Properties tab directly)

**Example Prompts to trigger skill**

- "What custom properties do I have available?"
- "Show me the user attributes I can target"
- "Does a 'subscription_tier' property exist?"
- "Help me create a new custom property"

**Constraints**

- Must show all existing properties from the page with types, not just those relevant to the current use case
- Must explain what each property type means (string, boolean, number, etc.) if asked
- Must guide property creation using the "Add Property" button on the page if a needed property does not exist
- Must not suggest targeting on properties that are not being sent by the SDK

**Acceptance Criteria**

The user is on the Target Groups > Properties tab. The assistant has read and presented all available custom properties from the page with their types. The relevant properties for the targeting criteria have been identified and confirmed. If new properties are needed, the user knows how to add them using the "Add Property" button on the current page.
