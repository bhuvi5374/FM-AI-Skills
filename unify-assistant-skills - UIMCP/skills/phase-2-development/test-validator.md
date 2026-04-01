**Name:** Bhuvi
**Date:** 2026-04-02
**Product Area:** Unify Feature Management — Development

## Prompts & Interactions

**User Goal/Problem being addressed**

I've run through my flag tests and want a final check to make sure I haven't missed anything critical before declaring the flag ready for deployment.

**Conversation Flow (Happy Path)**

1. User is on the **Flags > (flag detail) > Impressions tab** in the Unify FM UI and asks "Is my flag ready to deploy?" or "Did I test everything?"
2. Assistant reads the Impressions tab data visible on the page: impression counts per environment, which environments have recorded impressions (indicating the flag was evaluated), any anomalies or zero-impression environments shown
3. Assistant calls `flags_configurations_list` to retrieve the flag's full configuration — environments, targeting rules, and rollout settings — to cross-check against what has been tested
4. Assistant reviews the test checklist from test-scenario-generator and asks the user to confirm which items are complete
5. Assistant checks that both ON and OFF state impressions appear in the Local environment data shown on the Impressions tab
6. Assistant checks that state transition scenarios were covered (user confirms)
7. Assistant checks that targeting scenarios were tested if targeting rules are shown in `flags_configurations_list` results
8. If all critical items are complete and the Impressions tab shows activity in the expected environments, assistant declares the flag ready for deployment
9. If critical items are missing or the Impressions tab shows no activity where expected, assistant blocks the deployment declaration and explains what still needs to be done

**Context Requirements (Platform APIs/MCP Tools Used)**

- **UI Page:** Flags > (flag detail) > Impressions tab
- **Page content to use:** The impression count data on the Impressions tab — impressions per environment, first/last impression timestamps, and any environment with zero impressions shown
- **Unify MCP Server:** `flags_configurations_list` (params: applicationId) — used to retrieve the flag's full configuration (environments, targeting rules, rollout settings) to cross-check against the testing checklist

**Example Prompts to trigger skill**

- "Is my flag ready to deploy?"
- "Did I test everything I need to?"
- "Validate my testing is complete"
- "Sign off on this flag for deployment"

**Constraints**

- Must block deployment declaration if the OFF state was not tested (no OFF-state impressions expected or user has not confirmed)
- Must block deployment declaration if critical user flows were not tested
- Must require confirmation from the user for each checklist item — must not auto-pass items
- Must use the Impressions tab data to corroborate what the user reports was tested
- Must provide a clear list of what is still missing if the flag is not ready

**Acceptance Criteria**

The user is on the flag's Impressions tab. `flags_configurations_list` has confirmed the flag configuration. The Impressions tab shows activity in the expected environments. The assistant has verified that both ON and OFF states, all critical user flows, state transitions, and targeting scenarios have been tested and confirmed by the user. The flag is declared ready for deployment with a clear summary of what was tested.
