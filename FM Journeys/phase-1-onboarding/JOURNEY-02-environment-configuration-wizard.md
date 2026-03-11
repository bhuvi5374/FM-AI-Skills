# Journey: Environment Configuration Wizard

## Overview
**Goal:** Set up all environments (development, staging, production, etc.) with proper configuration, SDK keys, and defaults for feature flags.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Help me set up my environments"
- "I need to configure dev, staging, and prod"
- "How do I create environments in CloudBees FM?"
- "Set up environment structure for my team"

**Success Criteria:**
- All necessary environments created
- Each environment has unique SDK key
- Environment-specific defaults configured
- Environment hierarchy understood (dev → staging → prod)
- SDK keys securely stored
- Team understands environment best practices

## User Persona
- **Role:** Developer, DevOps Engineer, Platform Engineer (First-time FM user)
- **Pain Point:**
  - Confused about environment setup best practices
  - Risk of using same config across all environments
  - Unclear how many environments to create
  - Worried about accidentally testing in production
  - Need to match existing deployment pipeline structure

- **Current Manual Process:**
  1. Trial and error with environment setup
  2. Copy SDK keys to multiple places manually
  3. Confusion about environment inheritance
  4. Recreating environments after mistakes

  Time: 30-60 minutes of setup confusion

## Agentic Happy Path

### Step 1: Discovery - Understand Current Architecture
**What the agent does:**
- Ask about user's deployment pipeline
- Identify how many environments exist (CI, dev, staging, pre-prod, prod, etc.)
- Understand naming conventions used
- Determine if environments match git branches

**Context needed:**
- Common environment patterns (2-tier, 3-tier, 5-tier)
- User's tech stack (K8s, AWS, Azure, etc.)
- Team size and structure
- Compliance requirements (separate prod access)

**Example:**
```
Agent: "What deployment stages does your team use?"
User: "We have dev, staging, and production"
Agent: "Do developers test locally before dev?"
User: "Yes, local development"
Agent: "Recommended: Local, Development, Staging, Production"
```

### Step 2: Environment Planning
**What the agent does:**
- Suggest environment structure based on user's needs
- Explain purpose of each environment
- Recommend naming conventions
- Explain environment-specific flag behaviors

**Context needed:**
- Best practices for environment naming
- How flags behave differently per environment
- Common pitfalls (too many or too few environments)
- Environment inheritance concepts

**Example:**
```
Recommended Structure:
1. Local - Developer machines (SDK key: local_xxx)
2. Development - Shared dev server (SDK key: dev_xxx)
3. Staging - Pre-production testing (SDK key: staging_xxx)
4. Production - Live users (SDK key: prod_xxx)

Each environment:
- Has separate flag default values
- Independent rollout configurations
- Isolated from other environments
```

### Step 3: Create Environments
**What the agent does:**
- Create each environment in CloudBees FM
- Generate unique SDK keys per environment
- Set environment descriptions
- Configure environment order/hierarchy
- Set up environment-specific settings

**Context needed:**
- CloudBees FM API access
- Naming convention validation
- SDK key generation
- Environment metadata (color coding, icons)

**Example:**
```
Creating Environments:

✓ Local (Development)
  SDK Key: local_a1b2c3d4e5f6
  Default: Flags OFF by default

✓ Development (Shared)
  SDK Key: dev_x7y8z9a0b1c2
  Default: Flags ON for testing

✓ Staging (Pre-prod)
  SDK Key: staging_m3n4o5p6q7r8
  Default: Matches production settings

✓ Production (Live)
  SDK Key: prod_s9t0u1v2w3x4
  Default: Flags OFF, explicit rollout needed
```

### Step 4: Configure Environment Defaults
**What the agent does:**
- Set default flag values per environment
- Configure rollout policies (auto-rollout vs manual)
- Set up approval requirements per environment
- Configure environment variables if needed

**Context needed:**
- Risk tolerance per environment
- Approval workflow requirements
- Testing vs production behavior expectations

**Example:**
```
Environment Defaults:

Local:
- New flags: OFF by default
- Approvals: Not required
- Testing mode: Enabled

Development:
- New flags: ON by default (for testing)
- Approvals: Not required
- Auto-rollout: 100% immediately

Staging:
- New flags: Follow production config
- Approvals: Optional
- Test production scenarios

Production:
- New flags: OFF by default
- Approvals: REQUIRED
- Manual rollout only
```

### Step 5: Secure Key Distribution & Documentation
**What the agent does:**
- Provide secure methods to distribute SDK keys
- Generate environment setup documentation
- Create .env.example files for each environment
- Provide code snippets for SDK initialization
- Set up key rotation reminders

**Context needed:**
- Team's secret management system (AWS Secrets, Vault, etc.)
- Programming language/SDK being used
- CI/CD integration points
- Security best practices

**Example:**
```
SDK Key Distribution:

1. NEVER commit SDK keys to git
2. Store in environment variables:
   - Local: .env.local (gitignored)
   - Dev/Staging/Prod: Secret manager

3. Code Example (Node.js):
   const Rox = require('rox-browser');
   Rox.setup(process.env.CLOUDBEES_SDK_KEY);

4. CI/CD Integration:
   - GitHub Actions: Use secrets
   - Jenkins: Use credentials plugin

Documentation created: ENVIRONMENTS.md
```

## Required Skills

### Core Skills
- [ ] **environment-planner** - Recommends environment structure
  - Analyze user's deployment pipeline
  - Suggest appropriate number of environments
  - Recommend naming conventions
  - Explain environment purposes

- [ ] **environment-creator** - Creates environments via FM API
  - Call CloudBees FM API to create environments
  - Generate unique SDK keys
  - Set environment metadata (name, description, order)
  - Validate environment configuration

- [ ] **default-config-setter** - Configures environment-specific defaults
  - Set default flag values per environment
  - Configure rollout policies
  - Set approval requirements
  - Configure testing modes

- [ ] **sdk-key-manager** - Handles SDK key generation and security
  - Generate SDK keys securely
  - Provide key storage guidance
  - Create key rotation reminders
  - Generate .env templates

- [ ] **environment-documenter** - Creates setup documentation
  - Generate environment setup guide
  - Create SDK integration code samples
  - Document environment purposes
  - Provide troubleshooting tips

### Supporting Skills
- [ ] **secret-manager-integrator** - Guides integration with Vault, AWS Secrets, etc.
- [ ] **ci-cd-environment-guide** - Helps configure environments in CI/CD pipelines
- [ ] **environment-health-checker** - Validates environment configuration

## Constraints & Guardrails

**Must NOT do:**
- Create environments with guessable SDK keys
- Display SDK keys in logs or UI without masking
- Allow production keys in non-production environments
- Skip security warnings about key handling
- Create too many environments (complexity overhead)

**Must ALWAYS do:**
- Generate cryptographically secure SDK keys
- Warn about never committing keys to git
- Explain environment-specific flag behaviors
- Validate environment names follow conventions
- Provide key rotation guidance
- Create audit trail of environment changes

## Edge Cases

1. **Case:** User wants 10+ environments
   **Handling:** Warn about management complexity, suggest grouping, confirm intentional

2. **Case:** Team uses multiple git repos with shared environments
   **Handling:** Explain single environment set can serve multiple repos

3. **Case:** Different teams need different environment structures
   **Handling:** Allow per-app environment configuration, workspace segregation

4. **Case:** Migration from another FM system
   **Handling:** Import existing environment structure, map SDK keys

5. **Case:** Compliance requires complete prod isolation
   **Handling:** Set up separate production workspace, strict RBAC

6. **Case:** Developers need personal test environments
   **Handling:** Explain local development mode, personal SDK keys

7. **Case:** SDK key accidentally exposed in git
   **Handling:** Immediate key rotation, security incident guide

## Success Metrics
- [ ] Environments set up correctly in under 10 minutes
- [ ] Zero SDK keys committed to version control
- [ ] User understands environment purposes
- [ ] Proper separation between testing and production
- [ ] Team satisfaction: 4+ stars

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Follows: JOURNEY-01-repository-integration-setup
- Precedes: JOURNEY-03-integration-setup-assistant
- Enables: All flag creation and rollout journeys
