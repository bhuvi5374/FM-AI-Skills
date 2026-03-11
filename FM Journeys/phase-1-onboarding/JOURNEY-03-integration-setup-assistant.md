# Journey: Integration Setup Assistant (Jira, Slack, Webhooks)

## Overview
**Goal:** Connect CloudBees Feature Management with third-party tools (Jira, Slack, Microsoft Teams, Webhooks) to streamline workflows and improve team collaboration.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Help me integrate Jira with Feature Management"
- "Set up Slack notifications for flag changes"
- "How do I connect CloudBees FM to our tools?"
- "Configure webhooks for flag events"

**Success Criteria:**
- Selected integrations successfully connected
- Notifications working correctly
- Flags linked to Jira tickets
- Team receives appropriate alerts
- Webhooks delivering events as expected
- User understands how to use integrations

## User Persona
- **Role:** Developer, DevOps Engineer, Product Manager, Team Lead
- **Pain Point:**
  - Flag changes happen without team awareness
  - Hard to track which flags relate to which features/tickets
  - Context switching between tools is inefficient
  - Need automated notifications for flag events
  - Want to integrate FM into existing workflows

- **Current Manual Process:**
  1. Manually notify team in Slack about flag changes
  2. Keep separate notes linking flags to Jira tickets
  3. Check FM dashboard repeatedly for updates
  4. Copy-paste flag info into ticket comments

  Time: 5-10 minutes per flag change, ongoing overhead

## Agentic Happy Path

### Step 1: Identify Integration Needs
**What the agent does:**
- Ask which tools the team currently uses
- Understand what workflows need integration
- Identify use cases (notifications, linking, automation)
- Prioritize which integrations to set up first

**Context needed:**
- List of supported integrations in CloudBees FM
- Common integration use cases
- Team's collaboration tools
- User's access permissions to external tools

**Example:**
```
Agent: "What tools does your team use for collaboration?"
User: "Jira for tickets, Slack for chat"

Agent: "What would help most?"
User: "Link flags to Jira tickets, get Slack alerts for prod changes"

Agent: "Let's start with:
1. Jira integration (link flags to issues)
2. Slack notifications (production changes only)"
```

### Step 2: Jira Integration Setup
**What the agent does:**
- Guide user to generate Jira API token
- Configure Jira connection in CloudBees FM
- Map Jira projects to FM apps
- Test connection with sample query
- Show how to link flags to issues

**Context needed:**
- Jira instance URL (Cloud vs Server)
- Jira API authentication methods
- User's Jira permissions
- Project keys to integrate

**Example:**
```
Jira Setup:
1. Jira instance: https://your-company.atlassian.net
2. Create API token: Account Settings > Security > API Tokens
3. Enter credentials in CloudBees FM: Settings > Integrations > Jira
4. Test: Query issue FM-1234 ✓

Features enabled:
- Link flags to Jira issues
- View issue status on flag page
- Auto-populate flag descriptions from issues
```

### Step 3: Slack Integration Setup
**What the agent does:**
- Guide OAuth flow for Slack app installation
- Configure which channels receive notifications
- Set up notification filters (environments, flag types, events)
- Customize notification format
- Test with sample notification

**Context needed:**
- Slack workspace URL
- User's Slack admin permissions
- Desired notification channels
- Notification frequency preferences

**Example:**
```
Slack Setup:
1. Install CloudBees FM Slack App to workspace
2. Authorize access (needs channels:write permission)
3. Select notification channels:
   - #feature-flags - All flag changes
   - #production-alerts - Production rollouts only
4. Configure filters:
   ✓ Production changes: Always notify
   ✓ Staging changes: Only >50% rollout
   ✓ Dev changes: Don't notify

Test: Sent test message to #feature-flags ✓
```

### Step 4: Webhook Configuration (Optional)
**What the agent does:**
- Explain webhook use cases
- Help user configure webhook endpoint
- Set up event subscriptions
- Configure authentication (signatures, tokens)
- Test webhook delivery
- Provide debugging tips

**Context needed:**
- User's webhook endpoint URL
- Events to subscribe to
- Authentication requirements
- Retry and error handling expectations

**Example:**
```
Webhook Setup:
Endpoint: https://your-api.com/webhooks/fm
Events to send:
✓ flag.rollout.changed
✓ flag.created
✓ flag.deleted
✓ approval.requested

Authentication: Bearer token (configured)
Test webhook: Sent flag.created event ✓
Response: 200 OK

Webhook is active for all environments.
```

### Step 5: Integration Validation & Usage Training
**What the agent does:**
- Make test changes to validate integrations
- Show examples of integrated workflows
- Provide usage best practices
- Create integration documentation
- Set up monitoring for integration health

**Context needed:**
- Integration test scenarios
- Team workflow examples
- Common troubleshooting issues

**Example:**
```
Integration Test:
1. Create test flag "test_integration_check"
2. Link to Jira issue FM-9999
3. Change rollout to 50%

Results:
✓ Jira link appears on flag page
✓ Slack notification sent to #feature-flags
✓ Webhook delivered to endpoint (200 OK)

All integrations working correctly!

Next steps:
- Link existing flags to Jira issues
- Customize Slack notification filters
- Monitor webhook deliveries in logs
```

## Required Skills

### Core Skills
- [ ] **integration-recommender** - Suggests relevant integrations
  - Analyze team's tool stack
  - Recommend integration priority
  - Explain use cases for each integration
  - Estimate setup time

- [ ] **jira-connector** - Sets up Jira integration
  - Guide API token generation
  - Configure Jira connection
  - Test Jira API connectivity
  - Map projects to FM apps
  - Show flag-to-issue linking

- [ ] **slack-connector** - Sets up Slack integration
  - Guide OAuth app installation
  - Configure notification channels
  - Set up notification filters
  - Customize message format
  - Test notification delivery

- [ ] **webhook-configurator** - Sets up webhook integration
  - Configure webhook endpoint
  - Set up event subscriptions
  - Configure authentication
  - Test webhook delivery
  - Provide debugging guidance

- [ ] **integration-tester** - Validates integrations work
  - Create test scenarios
  - Execute integration tests
  - Verify event delivery
  - Generate test reports

- [ ] **integration-documenter** - Creates usage documentation
  - Document integration configurations
  - Provide workflow examples
  - Create troubleshooting guide

### Supporting Skills
- [ ] **ms-teams-connector** - Microsoft Teams integration setup
- [ ] **github-actions-connector** - GitHub Actions integration
- [ ] **datadog-connector** - Monitoring tool integration
- [ ] **integration-health-monitor** - Ongoing integration monitoring

## Constraints & Guardrails

**Must NOT do:**
- Store integration credentials insecurely
- Send sensitive flag data to public channels
- Enable integrations without user confirmation
- Spam Slack channels with excessive notifications
- Proceed if user lacks admin permissions to external tools

**Must ALWAYS do:**
- Validate credentials before enabling integration
- Explain what data will be shared with external tools
- Provide option to filter notifications
- Test integrations before declaring success
- Show user how to disable integrations later
- Respect rate limits on external APIs
- Provide clear error messages for failed integrations

## Edge Cases

1. **Case:** Jira Server (self-hosted) instead of Cloud
   **Handling:** Adjust authentication method, verify network access, provide Server-specific docs

2. **Case:** Slack workspace has strict app approval process
   **Handling:** Generate admin request template, explain required permissions

3. **Case:** Multiple Slack workspaces in organization
   **Handling:** Allow per-workspace configuration, support multiple connections

4. **Case:** Webhook endpoint requires IP allowlisting
   **Handling:** Provide CloudBees FM IP ranges, guide firewall configuration

5. **Case:** User wants notifications only for specific flag patterns
   **Handling:** Configure advanced filters with regex/wildcard matching

6. **Case:** Integration credentials expire
   **Handling:** Set up expiration alerts, provide renewal guidance

7. **Case:** Too many Slack notifications causing alert fatigue
   **Handling:** Recommend filter tuning, suggest digest notifications

## Success Metrics
- [ ] Integration setup completed in under 10 minutes per tool
- [ ] 100% notification delivery success rate
- [ ] Zero security issues with credentials
- [ ] Team finds integrations useful (4+ star rating)
- [ ] Reduced context switching between tools
- [ ] Increased flag-to-Jira ticket linking (>80% of flags)

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Follows: JOURNEY-02-environment-configuration-wizard
- Precedes: JOURNEY-04-first-flag-creation-tutorial
- Enhances: All operational journeys with better visibility
