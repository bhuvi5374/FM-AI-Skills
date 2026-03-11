# CloudBees Feature Management - User Journeys

**Version:** 1.0
**Created:** March 11, 2026
**Purpose:** Public-facing user journeys for CloudBees Feature Management workflows

---

## Overview

This repository contains 19 comprehensive user journeys organized by the feature flag lifecycle. Each journey describes a complete workflow from user trigger to success, with detailed required skills that will inform SKILL.md creation.

---

## Journey Organization

Journeys are organized into 6 lifecycle phases:

### Phase 1: Onboarding & Initial Setup (4 journeys)
New customer setup and first-time user experience

### Phase 2: Active Development (3 journeys)
Creating and testing flags during development

### Phase 3: Rollout & Release Management (3 journeys)
Safe production rollouts with monitoring

### Phase 4: Monitoring & Day-to-Day Operations (4 journeys)
Ongoing flag management and troubleshooting

### Phase 5: Cleanup & Technical Debt Management (3 journeys)
Removing stale flags and dead code

### Phase 6: Advanced Operations (2 journeys)
Complex scenarios and migrations

---

## Complete Journey Index

### Phase 1: Onboarding & Initial Setup

#### JOURNEY-01: Repository Integration Setup
**File:** `phase-1-onboarding/JOURNEY-01-repository-integration-setup.md`
**Goal:** Connect code repository to CloudBees FM for code reference tracking
**User:** Developer, DevOps Engineer (First-time user)
**Time Saved:** Enables all code analysis features
**Skills Required:** 5 core + 3 supporting

#### JOURNEY-02: Environment Configuration Wizard
**File:** `phase-1-onboarding/JOURNEY-02-environment-configuration-wizard.md`
**Goal:** Set up environments (dev, staging, prod) with SDK keys
**User:** Developer, Platform Engineer
**Time Saved:** 30-60 minutes setup confusion → 10 minutes
**Skills Required:** 5 core + 3 supporting

#### JOURNEY-03: Integration Setup Assistant
**File:** `phase-1-onboarding/JOURNEY-03-integration-setup-assistant.md`
**Goal:** Connect Jira, Slack, Webhooks for workflow integration
**User:** Developer, Product Manager, Team Lead
**Time Saved:** 5-10 minutes per flag change → automated
**Skills Required:** 6 core + 4 supporting

#### JOURNEY-04: First Flag Creation Tutorial
**File:** `phase-1-onboarding/JOURNEY-04-first-flag-creation-tutorial.md`
**Goal:** Guide new user through complete first flag experience
**User:** Developer (New to FM)
**Time Saved:** 1-2 hours confusion → 20 minutes
**Skills Required:** 5 core + 3 supporting

---

### Phase 2: Active Development

#### JOURNEY-05: New Flag Setup Wizard
**File:** `phase-2-development/JOURNEY-05-new-flag-setup-wizard.md`
**Goal:** Streamline flag creation with best practices
**User:** Developer, Product Manager
**Time Saved:** 10-15 minutes → 3 minutes
**Skills Required:** 6 core + 4 supporting

#### JOURNEY-06: Target Group Builder
**File:** `phase-2-development/JOURNEY-06-target-group-builder.md`
**Goal:** Create effective user segmentation for targeting
**User:** Product Manager, Developer, QA Engineer
**Time Saved:** 30-60 minutes → 5 minutes
**Skills Required:** 5 core + 5 supporting

#### JOURNEY-07: Flag Testing Assistant
**File:** `phase-2-development/JOURNEY-07-flag-testing-assistant.md`
**Goal:** Thoroughly test flags locally before deploying
**User:** Developer, QA Engineer
**Time Saved:** Prevents production issues
**Skills Required:** 5 core + 4 supporting

---

### Phase 3: Rollout & Release Management

#### JOURNEY-08: Gradual Rollout Planner
**File:** `phase-3-rollout/JOURNEY-08-gradual-rollout-planner.md`
**Goal:** Create safe incremental rollout plan with checkpoints
**User:** Developer, Product Manager, DevOps Engineer
**Time Saved:** Hours of planning → 10 minutes
**Skills Required:** 5 core + 5 supporting

#### JOURNEY-09: Rollout Impact Analyzer
**File:** `phase-3-rollout/JOURNEY-09-rollout-impact-analyzer.md`
**Goal:** Predict impact before changing rollout percentage
**User:** Developer, Product Manager, DevOps Engineer
**Time Saved:** 10-20 minutes uncertainty → 2 minutes analysis
**Skills Required:** 6 core + 4 supporting

#### JOURNEY-10: Flag Approval Request Assistant
**File:** `phase-3-rollout/JOURNEY-10-flag-approval-request-assistant.md`
**Goal:** Streamline approval requests with complete context
**User:** Developer, Product Manager
**Time Saved:** 1-3 days → 4-8 hours
**Skills Required:** 6 core + 4 supporting

---

### Phase 4: Monitoring & Day-to-Day Operations

#### JOURNEY-11: Flag Health Audit
**File:** `phase-4-operations/JOURNEY-11-flag-health-audit.md`
**Goal:** Generate comprehensive health report for all flags
**User:** Engineering Manager, Tech Lead, DevOps Engineer
**Time Saved:** 2-4 hours → 10 minutes
**Skills Required:** 5 core + 4 supporting

#### JOURNEY-12: Flag Performance Analyzer
**File:** `phase-4-operations/JOURNEY-12-flag-performance-analyzer.md`
**Goal:** Deep dive into flag metrics and trends
**User:** Product Manager, Engineering Manager, Data Analyst
**Time Saved:** 30-60 minutes → 2 minutes
**Skills Required:** 5 core + 4 supporting

#### JOURNEY-13: Flag Configuration Validator
**File:** `phase-4-operations/JOURNEY-13-flag-configuration-validator.md`
**Goal:** Diagnose and fix flag configuration issues
**User:** Developer, QA Engineer, Support Engineer
**Time Saved:** 30-120 minutes → 10 minutes
**Skills Required:** 5 core + 4 supporting

#### JOURNEY-14: Flag Documentation Generator
**File:** `phase-4-operations/JOURNEY-14-flag-documentation-generator.md`
**Goal:** Auto-generate comprehensive flag documentation
**User:** Engineering Manager, Tech Writer, Developer
**Time Saved:** 30-60 minutes per flag → 2 minutes
**Skills Required:** 5 core + 4 supporting

---

### Phase 5: Cleanup & Technical Debt Management

#### JOURNEY-15: Flag Cleanup Assistant
**File:** `phase-5-cleanup/JOURNEY-15-flag-cleanup-assistant.md`
**Goal:** Remove stale flags and dead code paths
**User:** Developer, Tech Lead, DevOps Engineer
**Time Saved:** 30-60 minutes → 10 minutes per flag
**Skills Required:** 6 core + 4 supporting

#### JOURNEY-16: Dead Code Removal Assistant
**File:** `phase-5-cleanup/JOURNEY-16-dead-code-removal-assistant.md`
**Goal:** Guide manual removal of dead code branches
**User:** Developer
**Time Saved:** 15-30 minutes → guided process
**Skills Required:** 5 core + 4 supporting

#### JOURNEY-17: Orphaned Flag Detector
**File:** `phase-5-cleanup/JOURNEY-17-orphaned-flag-detector.md`
**Goal:** Find flags in FM with no code references
**User:** Engineering Manager, DevOps Engineer, Tech Lead
**Time Saved:** Never done → automated detection
**Skills Required:** 5 core + 4 supporting

---

### Phase 6: Advanced Operations

#### JOURNEY-18: Flag Dependency Mapper
**File:** `phase-6-advanced/JOURNEY-18-flag-dependency-mapper.md`
**Goal:** Visualize flag dependencies and safe cleanup order
**User:** Tech Lead, Engineering Manager, Senior Developer
**Time Saved:** Hours investigation → minutes
**Skills Required:** 5 core + 4 supporting

#### JOURNEY-19: Flag Migration Helper
**File:** `phase-6-advanced/JOURNEY-19-flag-migration-helper.md`
**Goal:** Migrate from another FM platform to CloudBees
**User:** Engineering Manager, Platform Engineer
**Time Saved:** Weeks-months → 2-4 weeks structured
**Skills Required:** 6 core + 5 supporting

---

## Next Steps

### For Skill Creation:
Each journey file contains a **"Required Skills"** section that lists:
- Core skills (essential)
- Supporting skills (optional/enhancing)
- Skill descriptions and purposes

Use these to create individual SKILL.md files.

### For Implementation:
1. Review journeys by phase
2. Prioritize based on customer needs
3. Create SKILL.md files for required skills
4. Implement skills in AI agents
5. Test with users (3 internal + 5 external per journey)

### For Users:
Browse journeys by lifecycle phase to find workflows relevant to your current stage with feature flags.

---

## Document Format

Each JOURNEY.md file contains:
- **Overview** - Goal, trigger, success criteria
- **User Persona** - Role, pain points, current process
- **Agentic Happy Path** - Step-by-step workflow (5 steps)
- **Required Skills** - Core and supporting skills needed
- **Constraints & Guardrails** - What must/must not be done
- **Edge Cases** - Unusual scenarios and handling
- **Success Metrics** - Measurable outcomes

---

## Statistics

- **Total Journeys:** 19
- **Total Core Skills:** ~100 unique skills
- **Lifecycle Phases:** 6
- **User Roles Covered:** 10+
- **Time Savings:** Hundreds of hours per team per year

---

**Repository:** Public (for customer use)
**Maintained By:** Bhuvanesh Gunasekaran (Product Manager - Feature Management)
**GitHub:** https://github.com/bhuvi5374/FM-AI-Skills
**Documentation:** Each journey is self-contained and ready for AI agent implementation

---

**Last Updated:** March 11, 2026
