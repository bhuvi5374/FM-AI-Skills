**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I want automated test code for my feature flag that I can add to my test suite, covering both ON and OFF states, so regressions are caught automatically in CI/CD.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Overview tab** in the Unify FM UI and asks "Write automated tests for this flag"
2. Assistant reads the flag name and type visible on the Overview tab
3. Assistant calls `flags_configurations_list` to retrieve the full flag configuration — confirming the exact flag name, type, and environment defaults to use in the generated test code
4. Assistant asks for the user's testing framework (Jest, pytest, JUnit, Go testing, etc.) if not already known
5. Assistant generates automated test code for the ON state test cases, using the exact flag name from the page/MCP call
6. Assistant generates automated test code for the OFF state test cases
7. Assistant includes the SDK flag override setup in the test setup (beforeEach / setUp) and teardown in each test (afterEach / tearDown)
8. User receives the test code ready to copy into their test suite

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Overview tab
- **Page content to use:** The flag name and flag type displayed on the Overview tab
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the exact flag name and type to ensure the generated test code uses the correct flag identifier

**Example Prompts to trigger skill**

- "Write automated tests for this flag"
- "Generate Jest tests for ON and OFF states"
- "Create unit tests for my feature flag"
- "Give me the test code for flag testing"

**Constraints**

- Must generate tests for both ON and OFF states in every output
- Must include setup (flag override) and teardown (reset flag) in each test
- Must use the user's actual testing framework — not a generic one
- Must use the exact flag name from the Overview tab and confirmed by `flags_configurations_list` — not a placeholder

**Acceptance Criteria**

The user is on the flag's Overview tab. `flags_configurations_list` has confirmed the exact flag name and type. The user receives working automated test code in their testing framework for both ON and OFF states. Each test correctly sets up the flag override using the exact flag name, tests the expected behavior, and tears down the override. The tests are ready to add to the CI/CD pipeline.
