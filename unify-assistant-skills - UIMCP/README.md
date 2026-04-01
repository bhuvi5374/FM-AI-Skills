# Unify Assistant Skills

Skill definitions for the Unify AI Assistant — structured instructions that describe how the assistant handles specific parts of the CloudBees platform.

## Structure

* `skill-definition.md` — Template for defining new skills

## Creating a New Skill

1. Copy `skill-definition.md` to a new file under the appropriate functional area (e.g. `skills/ci-cd/my-skill.md`)
2. Fill in all sections, replacing the placeholder text with your skill's details
3. Open a PR for review

## Skill Definitions vs Agent Skills

This repository contains **skill definitions** — internal documents that capture the requirements for a skill (user goal, conversation flow, context requirements, acceptance criteria). They are used by the AI team to understand what a skill should do.

An **[Agent Skill](https://agentskills.io)** is something different — it's an open, portable format (`SKILL.md` + supporting files) that agents like Claude Code, Cursor, and others can discover and execute directly. Agent Skills are structured for machine consumption with YAML frontmatter, instructions, and optional scripts/references.

**When to use which:**

* **Skill definition** — You're a PM or domain expert describing a new capability for the assistant. You want to capture the what and why.
* **Agent Skill** — You're implementing a skill that an agent can load and execute autonomously. You're writing the how in a format agents understand.

In practice, a skill definition is the input; an Agent Skill may be the output.

## References

* [AI Assistant Skills](https://cloudbees.atlassian.net/wiki/spaces/teamc78bbf58f3604a138443df3138f4dc1c/pages/6237552804/AI+Assistant+Skills) — Process documentation
* [Assistant Skill Template](https://cloudbees.atlassian.net/wiki/spaces/teamc78bbf58f3604a138443df3138f4dc1c/pages/6244073492/Assistant+Skill+Template) — Original Confluence template
