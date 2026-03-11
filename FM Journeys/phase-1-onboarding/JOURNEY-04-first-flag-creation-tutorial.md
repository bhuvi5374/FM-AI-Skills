# Journey: First Flag Creation Tutorial

## Overview
**Goal:** Guide a new user through creating their first feature flag end-to-end, from FM setup to code integration to testing, building confidence and understanding.

**Trigger:**
User asks the AI assistant with natural language queries such as:
- "Help me create my first feature flag"
- "I'm new to FM, how do I get started with a flag?"
- "Walk me through creating a simple flag"
- "Tutorial for creating a feature flag"

**Success Criteria:**
- First flag created successfully in CloudBees FM
- Flag integrated into user's codebase
- User tested flag locally (ON and OFF states)
- Flag deployed to development environment
- User understands complete flag lifecycle
- Confidence to create more flags independently

## User Persona
- **Role:** Developer (New to Feature Management)
- **Pain Point:**
  - Never used feature flags before
  - Overwhelmed by FM documentation
  - Unsure where to start
  - Fear of breaking something
  - Need hands-on learning with real example

- **Current Manual Process:**
  - Read documentation without practical context
  - Trial and error with flag creation
  - Confusion about flag types and when to use each
  - Uncertainty about code integration

  Time: 1-2 hours of confusion and research

## Agentic Happy Path

### Step 1: Choose a Simple Use Case
**What the agent does:**
- Suggest beginner-friendly flag use case
- Explain different flag types (boolean, string, number)
- Help user pick appropriate flag for their scenario
- Set expectations for tutorial scope

**Context needed:**
- User's programming language
- Simple example relevant to user's domain
- Understanding of flag types

**Example:**
```
Agent: "Let's create your first flag together!
What would you like to control with a flag?"

User: "Maybe show/hide a new button in our UI?"

Agent: "Perfect! That's a boolean flag (ON/OFF).
We'll create 'show_new_dashboard_button' and you'll
learn how to:
1. Create the flag in CloudBees FM
2. Add it to your code
3. Test it works
4. Deploy it"
```

### Step 2: Create Flag in CloudBees FM
**What the agent does:**
- Guide user to flag creation page
- Help choose flag name (naming conventions)
- Select flag type (boolean for this example)
- Set description and tags
- Configure default values per environment
- Explain each field's purpose

**Context needed:**
- FM UI navigation
- Naming best practices
- Environment configuration from previous setup

**Example:**
```
Creating Flag in CloudBees FM:

1. Navigate to: Flags > Create New Flag

2. Flag Details:
   Name: show_new_dashboard_button
   Type: Boolean
   Description: "Controls visibility of redesigned dashboard button"
   Tags: tutorial, ui, dashboard

3. Default Values:
   - Local: ON (so you can test easily)
   - Development: ON (team can test)
   - Staging: OFF (matches prod initially)
   - Production: OFF (safe default)

4. Click "Create Flag" ✓

Your flag is created! SDK will now recognize it.
```

### Step 3: Integrate Flag in Code
**What the agent does:**
- Provide SDK code snippet for user's language
- Show where to place flag check in code
- Explain if/else logic for flag states
- Help user add flag to their codebase
- Review code changes together

**Context needed:**
- User's programming language and framework
- SDK already initialized (from env setup)
- Location in code where flag should be checked

**Example:**
```
Add Flag to Your Code (React example):

1. Import the SDK (if not already):
   import { Flags } from 'cloudbees-fm-sdk';

2. Check the flag in your component:
   function Dashboard() {
     const showNewButton = Flags.showNewDashboardButton.isEnabled();

     return (
       <div>
         <h1>Dashboard</h1>
         {showNewButton && (
           <button>New Dashboard</button>
         )}
       </div>
     );
   }

3. Save the file

When flag is ON: Button appears
When flag is OFF: Button hidden
```

### Step 4: Test Locally
**What the agent does:**
- Guide user to test flag in local environment
- Show how to verify flag value
- Test both ON and OFF states
- Explain local overrides for testing
- Troubleshoot if flag doesn't work

**Context needed:**
- Local SDK configuration
- How to verify SDK connection
- Common testing issues

**Example:**
```
Testing Your Flag Locally:

1. Start your application locally
2. Check SDK console logs:
   ✓ Connected to CloudBees FM
   ✓ Loaded flag: show_new_dashboard_button = true

3. Open your app in browser:
   ✓ New Dashboard Button is visible

4. Test OFF state:
   - Go to FM UI > Flags > show_new_dashboard_button
   - Change Local environment to OFF
   - Refresh your app
   ✓ Button is now hidden

Both states work! Your flag is functioning correctly.
```

### Step 5: Deploy & Next Steps
**What the agent does:**
- Guide committing code changes
- Explain deployment to dev environment
- Show how to change flag values in FM
- Explain gradual rollout concepts (preview of future)
- Celebrate success and suggest next flags

**Context needed:**
- Git workflow
- Deployment process
- Rollout concepts for future learning

**Example:**
```
Deploy Your First Flag:

1. Commit your code:
   git add .
   git commit -m "Add show_new_dashboard_button flag"
   git push

2. Deploy to Development:
   (Your normal deployment process)

3. Verify in Dev environment:
   - Flag value: ON (as configured)
   - Button appears in dev environment ✓

Congratulations! You've created and deployed
your first feature flag!

What you learned:
✓ Create flags in CloudBees FM
✓ Integrate flags in code
✓ Test flags locally
✓ Deploy flag-controlled features

Next steps:
- Try a gradual rollout (0% → 50% → 100%)
- Learn about target groups
- Explore flag analytics
```

## Required Skills

### Core Skills
- [ ] **use-case-recommender** - Suggests appropriate first flag scenario
  - Analyze user's domain and tech stack
  - Recommend simple, safe use case
  - Explain flag type selection
  - Set realistic expectations

- [ ] **flag-creator-guide** - Walks through FM flag creation
  - Navigate FM UI step-by-step
  - Explain each form field
  - Validate flag name conventions
  - Configure environment defaults
  - Explain flag metadata (tags, description)

- [ ] **code-integration-tutor** - Teaches SDK integration
  - Generate code snippet for user's language
  - Explain flag check logic (if/else)
  - Show where to place code
  - Review code changes
  - Explain SDK patterns

- [ ] **local-testing-guide** - Guides local flag testing
  - Verify SDK connection
  - Test both flag states (ON/OFF)
  - Explain local overrides
  - Troubleshoot common issues
  - Validate flag behavior

- [ ] **deployment-guide** - Helps deploy first flag
  - Git commit guidance
  - Deployment verification
  - Environment-specific testing
  - Success celebration

### Supporting Skills
- [ ] **flag-naming-advisor** - Suggests good flag names
- [ ] **sdk-troubleshooter** - Debugs SDK connection issues
- [ ] **next-steps-recommender** - Suggests follow-up learning

## Constraints & Guardrails

**Must NOT do:**
- Create complex flag scenarios for first-time users
- Use production environment for first flag test
- Skip explaining flag state behavior (ON/OFF)
- Rush through steps without confirmation
- Use technical jargon without explanation

**Must ALWAYS do:**
- Start with simplest possible flag (boolean)
- Test in local/dev environments only
- Verify each step before proceeding
- Celebrate small wins (build confidence)
- Explain the "why" not just the "how"
- Provide clear error messages
- Check user understanding before moving forward

## Edge Cases

1. **Case:** SDK not initialized in user's code
   **Handling:** Backtrack to SDK setup, ensure environment configured

2. **Case:** User's local environment doesn't match FM configuration
   **Handling:** Debug environment mismatch, fix SDK key

3. **Case:** Flag doesn't appear to work (no visual change)
   **Handling:** Check SDK logs, verify flag name match, check cache

4. **Case:** User wants to start with complex multi-variant flag
   **Handling:** Explain boolean first, promise to cover advanced later

5. **Case:** User's codebase uses different FM SDK than expected
   **Handling:** Adapt code examples to their SDK version

6. **Case:** Corporate firewall blocks FM API
   **Handling:** Guide proxy configuration, work with IT

7. **Case:** User gets overwhelmed during tutorial
   **Handling:** Simplify further, offer to pause and resume later

## Success Metrics
- [ ] First flag created and deployed in under 20 minutes
- [ ] User successfully tests both flag states
- [ ] Zero production incidents from tutorial flag
- [ ] User confidence level: "I can create more flags" (95%+ yes)
- [ ] User proceeds to create second flag independently
- [ ] Tutorial satisfaction: 4.5+ stars

## Notes & Iterations
**Version:** 1.0
**Last Updated:** 2026-03-11
**Feedback:** First draft - needs validation with real users
**Related Journeys:**
- Follows: JOURNEY-03-integration-setup-assistant
- Precedes: JOURNEY-05-new-flag-setup-wizard (for subsequent flags)
- Foundation for: All future flag management journeys
