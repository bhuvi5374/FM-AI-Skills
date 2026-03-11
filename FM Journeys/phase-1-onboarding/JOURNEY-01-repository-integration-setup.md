# Journey: Repository Integration Setup

## Overview
**Goal:** Connect code repository (GitHub/GitLab/Bitbucket) to CloudBees Feature Management to enable code reference tracking and flag discovery.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Help me connect my GitHub repo to CloudBees FM"
- "How do I set up code repository integration?"
- "Link my codebase to Feature Management"
- "Enable code scanning for feature flags"

**Success Criteria:**
- Repository successfully connected to CloudBees FM
- Code scanning configured and running
- Flag references detected in codebase
- User understands what code references provide
- Integration validated with test scan

## User Persona
- **Role:** Developer, DevOps Engineer, Team Lead (First-time FM user)
- **Pain Point:**
  - New to CloudBees FM, unsure how to start
  - Wants visibility into where flags are used in code
  - Manual code searches are tedious and error-prone
  - Need to track flag usage across large codebases

- **Current Manual Process:**
  1. Manually search code with grep/IDE search for flag names
  2. Keep spreadsheet of flag locations
  3. Ask team members where flags are used
  4. Review PRs to find new flag additions

  Time: Ongoing maintenance burden

## Agentic Happy Path

### Step 1: Understand Repository Type
**What the agent does:**
- Ask user which code hosting platform they use
- Identify repository URL or name
- Check if repository is public or private
- Determine authentication method needed

**Context needed:**
- List of supported platforms (GitHub, GitLab, Bitbucket, Azure DevOps)
- User's access level to repository
- Whether organization has SSO/special auth requirements

**Example:**
```
Agent: "Which platform hosts your code?"
User: "GitHub"
Agent: "Is it github.com/your-org/repo-name?"
User: "Yes"
Agent: "Do you have admin access to this repo?"
```

### Step 2: Configure Access Permissions
**What the agent does:**
- Guide user to generate access token/SSH key
- Explain minimum permissions needed (read access to code)
- Help user create CloudBees FM integration in their git platform
- Validate credentials work

**Context needed:**
- Platform-specific permission requirements
- Token scopes needed (repo:read for GitHub)
- Webhook permissions if real-time sync desired
- CloudBees FM API access

**Example:**
```
For GitHub:
1. Go to Settings > Developer settings > Personal access tokens
2. Create token with "repo" scope (read access)
3. Copy token to CloudBees FM integration page
4. Test connection
```

### Step 3: Enable Code Scanning
**What the agent does:**
- Configure which branches to scan (main, develop, etc.)
- Set up flag detection patterns for user's languages
- Configure scan frequency (on commit, daily, manual)
- Explain what will be detected

**Context needed:**
- Programming languages used in repo
- Common flag naming patterns in codebase
- SDK being used (determines flag syntax)
- Performance considerations for large repos

**Example:**
```
Detected languages: JavaScript, Python
Flag patterns to scan:
- JavaScript: isFeatureEnabled('flag_name')
- Python: is_feature_enabled('flag_name')

Scan frequency: On every push to main branch
```

### Step 4: Initial Scan & Validation
**What the agent does:**
- Trigger first repository scan
- Monitor scan progress
- Display discovered flags and their locations
- Validate that known flags are detected
- Show code reference examples

**Context needed:**
- Expected scan duration based on repo size
- How to interpret scan results
- What to do if flags are missing

**Example:**
```
Scan Results:
- Total flags found: 47
- Code references: 312 locations
- Files with flags: 89

Sample: flag_new_checkout found in:
- src/checkout/processor.js:45
- src/checkout/validator.js:120
- tests/checkout.test.js:89
```

### Step 5: Completion & Education
**What the agent does:**
- Confirm integration is active
- Show user where to view code references in FM UI
- Explain how references stay up-to-date
- Provide next steps (creating flags, using references)
- Set up alerts for new flag additions (optional)

**Context needed:**
- FM UI navigation paths
- How often scans run
- How to manually trigger scans
- Where to troubleshoot issues

**Example:**
```
Integration Complete!

Next steps:
1. View code references on any flag's detail page
2. Scans run automatically on every push
3. Manual scan: Settings > Code Integration > Scan Now
4. Use references to track flag cleanup progress
```

## Required Skills

### Core Skills
- [ ] **platform-detector** - Identifies git platform type and capabilities
  - Detect GitHub vs GitLab vs Bitbucket vs Azure DevOps
  - Determine cloud vs self-hosted
  - Check API version compatibility

- [ ] **auth-credential-guide** - Guides token/SSH key generation
  - Platform-specific auth setup instructions
  - Scope/permission validation
  - Secure credential handling
  - Test connection functionality

- [ ] **code-scanner-configurator** - Sets up code scanning parameters
  - Language detection in repository
  - Flag pattern configuration by language/SDK
  - Branch selection (which to scan)
  - Scan frequency settings

- [ ] **initial-scan-runner** - Executes and monitors first code scan
  - Trigger repository scan via API
  - Monitor scan progress
  - Parse and display results
  - Validate expected flags found

- [ ] **code-reference-explainer** - Teaches user how to use code references
  - Navigate to reference views in FM UI
  - Interpret reference data
  - Use references for cleanup planning

### Supporting Skills
- [ ] **repo-size-analyzer** - Estimates scan time based on repo size
- [ ] **webhook-configurator** - Optional real-time sync setup
- [ ] **integration-health-checker** - Validates ongoing connection status

## Constraints & Guardrails

**Must NOT do:**
- Request more permissions than needed (avoid write access)
- Store git credentials insecurely
- Scan private repos without explicit user consent
- Enable webhooks without explaining what they do
- Proceed if user lacks necessary repository permissions

**Must ALWAYS do:**
- Validate credentials before proceeding
- Explain what data will be accessed (read-only code)
- Show user how to revoke access later
- Provide clear error messages if connection fails
- Respect rate limits on git platform APIs

## Edge Cases

1. **Case:** Repository is extremely large (>100k files)
   **Handling:** Warn about long scan time, suggest scanning specific paths/branches only

2. **Case:** Organization uses SSO with SAML
   **Handling:** Guide to create organization-approved token, link to admin documentation

3. **Case:** Multiple repositories need integration
   **Handling:** Allow batch setup, provide CLI option for scale

4. **Case:** Repository uses non-standard flag naming
   **Handling:** Allow custom regex patterns for flag detection

5. **Case:** User lacks admin access to repository
   **Handling:** Generate request template for admin, explain what's needed

6. **Case:** Monorepo with multiple projects
   **Handling:** Allow per-project flag configuration, separate scanning configs

7. **Case:** Repository has no flags yet
   **Handling:** Confirm integration works, explain flags will appear as code is added

## Success Metrics
- [ ] Integration completed in under 5 minutes
- [ ] 95%+ of existing flags detected in first scan
- [ ] Zero credential security issues
- [ ] User understands how to view code references
- [ ] User satisfaction: 4+ stars

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Follows into: JOURNEY-02-environment-configuration
- Enables: All cleanup and code analysis journeys
