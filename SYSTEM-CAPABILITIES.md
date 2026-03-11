# CloudBees Unify - Feature Management System Capabilities

**Source:** docs.cloudbees.com (Research Date: March 4, 2026)
**Researched By:** Claude for Bhuvi's FM AI Assistant Project

---

## 🏢 CloudBees Unify Platform Overview

CloudBees Unify is an enterprise DevSecOps platform that integrates multiple capabilities:
- Continuous Integration & Deployment
- Security & Compliance Scanning
- Feature Management (our focus)
- Analytics & Observability (DORA metrics)
- Release Orchestration

---

## 🚩 Feature Management Capabilities

### Flag Types
Users can create three types of flags:
1. **Boolean flags** - Simple on/off controls
2. **String flags** - Configuration values
3. **Number flags** - Numeric parameters

### Core Flag Operations (In UI)

#### Creating & Managing Flags
- Create flags in UI or via code
- Set flag default values per environment
- Edit flag configurations
- Archive/delete flags
- Organize flags into **flag groups**
- Multi-platform flag support

#### Release Control
- **Percentage rollouts** - Gradually release to X% of users
- **Scheduled rollouts** - Deploy at specific times
- **Flag dependencies** - Flag A depends on Flag B
- **Flag freeze levels** - Prevent changes during critical periods
- Toggle flags on/off instantly

### Targeting & Segmentation

#### User Targeting
- **Target groups** - Define specific user cohorts
- **Custom properties** - Segment by user attributes (e.g., email domain, subscription tier)
- **Flag context** - Contextual decision-making based on runtime data
- **Split releases** - A/B testing configurations

### Governance & Workflows

#### Approval System
- **Flag approval requests** - Changes require review
- **Approval workflows** - Multi-step review process
- **Audit logging** - Track all flag changes
- **Role-based access control** - Permissions at org and app levels

### Analytics & Monitoring

#### Observability
- **Flag impressions** - Track when flags are evaluated
- **Impression handlers** - Custom analytics integration
- **Configuration fetched handlers** - Monitor SDK sync
- **Flag health** tracking
- **Verbose logging** for debugging

### Integrations

#### Third-Party Tools
- **Jira** - Link flags to issues
- **Slack** - Notifications for flag changes
- **Microsoft Teams** - Team notifications
- **Jenkins** - CI/CD integration
- **Webhooks** - Custom integrations
- **Configuration as Code** - GitHub integration

### Multi-Environment Support
- Separate configurations per environment (dev, staging, prod)
- Environment-specific defaults
- Environment inventory management

### Developer Integration

#### SDKs Available
**Client-Side:**
- Android
- iOS
- JavaScript
- .NET
- React Native
- TypeScript

**Server-Side:**
- Go
- Java
- Node.js
- PHP
- Python
- Ruby

**Other:**
- JavaScript SSR (Server-Side Rendering)

#### Testing & Debugging
- Local testing overrides
- Microservices testing strategies
- Verbose logging
- Code reference detection

### Security Features
- **SAML SSO** - Single sign-on
- **MFA** - Multi-factor authentication
- **Secret mode** - Hide sensitive flags
- **Domain controls** - Restrict access
- **Proxy configuration** - Enterprise network support

---

## 📊 What Users Can Do Today

Based on the documentation, FM users can:

### 1. Flag Lifecycle Management
✅ Create flags (boolean, string, number)
✅ Configure flag defaults per environment
✅ Edit flag settings
✅ Archive old flags
✅ Delete flags (with caution)
✅ Organize flags into groups

### 2. Targeting & Rollouts
✅ Define target groups (user segments)
✅ Set custom properties for targeting
✅ Configure percentage rollouts (e.g., 10% → 50% → 100%)
✅ Schedule automated rollouts
✅ Create split tests (A/B tests)

### 3. Governance
✅ Request approval for flag changes
✅ Review and approve requests
✅ View audit history
✅ Manage user permissions
✅ Freeze flags during critical periods

### 4. Monitoring
✅ Track flag impressions (usage)
✅ Monitor flag health
✅ View flag activity/status
✅ Debug with verbose logging

### 5. Integrations
✅ Link flags to Jira tickets
✅ Receive Slack notifications
✅ Trigger webhooks on flag changes
✅ Manage flags via code (GitHub)

---

## ❓ What Users Struggle With (Inferred Pain Points)

Based on capabilities, likely challenges:

### Flag Cleanup
- **Problem:** Flags accumulate over time, no automated cleanup
- **Current Process:** Manual identification and removal
- **Gap:** No "stale flag" detection or automated cleanup

### Code Impact Analysis
- **Problem:** Hard to know where a flag is used in code
- **Current Process:** Manual code search
- **Gap:** No "code reference checker" to find all usages

### Flag Health
- **Problem:** Determining if a flag is still needed
- **Current Process:** Check impressions, rollout %, age manually
- **Gap:** No holistic "flag health score"

### Documentation
- **Problem:** Flags created without good descriptions
- **Current Process:** Manual documentation
- **Gap:** No auto-documentation from code or Jira

### Testing Impact
- **Problem:** Understanding test coverage for flagged code
- **Current Process:** Manual test analysis
- **Gap:** No flag-to-test mapping

---

## 🎯 AI Assistant Opportunity Areas

Based on system capabilities, the AI assistant can help with:

### High Value (Phase 1 - UI/Read Only)
1. **Stale Flag Identification** ✨
   - Query FM API for flags at 100% rollout
   - Calculate age at 100%
   - Recommend cleanup candidates

2. **Flag Health Reports**
   - Combine impressions + rollout % + age
   - Generate health scores
   - Alert on unused flags

3. **Approval Workflow Assistance**
   - Summarize pending approvals
   - Draft approval requests
   - Track approval status

### High Value (Phase 2 - Code Modification)
1. **Automated Flag Cleanup** ✨✨✨
   - Scan code for flag references
   - Remove dead code paths
   - Create cleanup PRs

2. **Code Impact Analysis**
   - Show all code locations using a flag
   - Identify test coverage
   - Map dependencies

3. **Auto-Documentation**
   - Generate flag docs from code
   - Update descriptions from Jira
   - Create usage guides

---

## 📝 Notes for Journey Creation

When building journeys, consider:
- Users interact via **FM dashboard in Unify UI**
- All data is accessible via **FM API**
- Flags have **multi-environment configs**
- **Approval workflows** are critical (governance)
- Users care about **percentage rollout** and **target groups**
- **Jira integration** is heavily used
- **Audit trail** is important (compliance)

---

**Last Updated:** March 4, 2026
**Next Steps:** Use this knowledge to refine journey definitions and identify additional high-value journeys
