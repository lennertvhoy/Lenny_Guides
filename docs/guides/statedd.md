# StateDD

StateDD is a repo-first project workflow.

## What StateDD does

Stores project status, backlog, decisions, validation, evidence, and handoffs in the repo.

## When to use it

- Any project where an AI agent writes or modifies code

## Basic setup

Add these files to your project repo:

```bash
AGENTS.md
STATUS.md
PROJECT_STATE.yaml
PROJECT_DNA.yaml
NEXT_ACTIONS.md
BACKLOG.md
WORKLOG.md
docs/EVIDENCE_LOG.md
evidence/
```

## Safety notes

!!! warning "Implemented is not validated"
    The agent must prove a change with tests, runtime proof, or screenshots.

## Common mistakes

- Accepting "I'm done" without evidence
- Letting the agent rewrite StateDD files to match its own claim

## What to do next

- Read the [Safety Boundaries page](../safety/boundaries.md)
