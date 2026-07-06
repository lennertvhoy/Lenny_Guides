# Boundaries

A coding agent is a fast junior colleague in your terminal. It needs context, limits, and review.

## The non-negotiables

- **Repo first.** The Git repo is the source of truth. Herdr, StudyDD, and StateDD only organize work around it.
- **Plan before build.** For any non-trivial task, the agent proposes a plan before changing files.
- **Evidence before closure.** A task is not done because the agent says so. You need tests, screenshots, logs, or a clean diff.
- **No secrets in chat or Git.** API keys, tokens, and passwords do not belong in prompts, screenshots, or commits.
- **One task at a time.** Parallel agents only when they do not touch the same files or decisions.
- **Human stays reviewer.** The agent proposes; you accept or reject.

## What not to let the agent do

```text
- Paste API keys into Discord or prompts
- Commit .env files
- Take screenshots where tokens are visible
- Expose SSH via router port forwarding
- Run force push
- Change passwords
- Read production data without permission
- Run shell commands without you understanding them
```

## What to do instead

```text
- Use .env.example without real secrets
- Keep .gitignore clean
- Use Tailscale for private access
- Use small branches
- Read git diff before committing
- Treat agent output as a proposal, not truth
- Ask for evidence before closure
```

## Example .gitignore

```gitignore
.env
.env.*
!.env.example
*.key
*.pem
secrets/
node_modules/
.venv/
```

## Phone vs laptop

- **Phone:** good for checking status, short approvals, reading handoffs.
- **Laptop:** required for large diffs, merge conflicts, security decisions, complex refactors.

## Remember

Repo first. Plan first. Evidence before closure.
