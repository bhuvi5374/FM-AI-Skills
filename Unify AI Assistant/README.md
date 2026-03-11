# FM AI Assistant - Critical User Journeys & Skills

**Owner:** Bhuvanesh Gunasekaran (Product Manager - Feature Management)
**Project:** CloudBees Unify - Feature Management AI Assistant
**Started:** March 4, 2026

## Overview

This project contains Critical User Journeys (CUJs) and Skills for AI agents to assist Feature Management users. Based on guidance from the AI team (Jason Burt / Jordan Matthiesen).

## 📁 Project Structure

```
ai-assistant/
├── README.md                    ← You are here
├── JOURNEY-TEMPLATE.md          ← Template for creating new journeys
├── SKILL-TEMPLATE.md            ← Template for creating new skills
├── journeys/
│   └── JOURNEY-flag-cleanup.md  ← First journey (complete)
└── skills/
    ├── SKILL-code-reference-checker.md  ← Finds flag references
    └── SKILL-flag-cleaner.md            ← Removes dead code
```

## 🎯 Completed Journeys

### 1. Flag Cleanup
**Status:** ✅ Complete (needs testing)
**Goal:** Automatically remove feature flags that are 100% rolled out for >14 days
**Skills Required:**
- code-reference-checker
- flag-cleaner

**Time Saved:** 30-60 minutes → ~5 minutes per flag

## 📋 Next Steps

1. **Review** - Read through the journey and skills
2. **Share** - Get feedback from CloudBees engineers
3. **Test** - Validate with real codebases
4. **Iterate** - Improve based on feedback
5. **Document more journeys** - Identify other FM pain points

## 📝 Testing Requirements

- [ ] 3 internal team members test the journey
- [ ] 5 external users provide feedback
- [ ] Document learnings and iterate
- [ ] Save to CloudBees git repo for AI Foundations team

## 🔗 Resources

- **Source Document:** PM Guide: Building With Skills (Feb 25, 2026)
- **Stakeholders:** Jason Burt, Jordan Matthiesen, AI Foundations team
- **Location:** `/Users/bhuvi/cloudbees/fm/ai-assistant/`

---

**Note:** This is a living document. Update as journeys evolve and new ones are created.
