# StateSpec

> **Naming:** StateSpec is the portable application specification. Stateware is
> the broader software category, and State-Centric Engineering is the method
> used to build it. `StateDD` remains a legacy compatibility name in existing
> repository paths, schemas, and links.

## One-line summary

StateSpec keeps your project's current status, decisions, backlog, validation evidence, and session handoffs inside the same Git repo where the agent works, so the repo is always the single source of truth.

## What it does

StateSpec defines the portable files, ownership rules, lifecycle contracts,
context compilation, validation, and evidence requirements for a Stateware
application. State-Centric Engineering is the practice of applying those
contracts. Instead of trusting a chat thread to remember what was built and why,
you store the important project state in plain-text files inside
`~/Projects/study-project`. You and the agent both read and update the same
files, so every session starts from the actual current state.

The file set is:

- `AGENTS.md` — rules for the agent in this project repo, for example "do not mark a task closed without evidence".
- `STATUS.md` — the current project status, open tasks, and blockers.
- `PROJECT_STATE.yaml` — machine-readable project state such as active phase, last session date, and health flags.
- `PROJECT_DNA.yaml` — stable project facts such as tech stack, conventions, and constraints.
- `NEXT_ACTIONS.md` — the exact next step the agent should take.
- `BACKLOG.md` — tasks and ideas for later.
- `WORKLOG.md` — a short log of what happened in each session.
- `docs/EVIDENCE_LOG.md` — a catalog of validation evidence such as test output, screenshots, or diffs.
- `evidence/` — folder that holds the actual evidence files.

The agent reads these files at the start of a session, proposes changes, and updates state files only when you approve.

## When to use it

Use StateSpec when you want the agent to build or change code over more than one session:

- Building a small application or script.
- Refactoring or adding features to an existing project.
- Debugging a non-trivial issue that needs several attempts.
- Any project where you need to prove that a change works before moving on.

It works best when every closed task has a matching piece of evidence.

## What you need

This guide uses `lenny` as the username on `studybox` and `student-phone` as your Android device. Replace them with your own names.

- A project machine called `studybox` with Git and a terminal installed.
- A user account `lenny` on `studybox`.
- A directory for the repo at `~/Projects/study-project`.
- Your Android phone `student-phone` with Termux, if you want to check status on the go.
- About ten minutes for the first setup.

## Install and verify

1. Create the repo directory and the evidence folder:

   ```bash
   mkdir -p ~/Projects/study-project/docs
   mkdir -p ~/Projects/study-project/evidence
   cd ~/Projects/study-project
   ```

   `mkdir -p` creates parent directories as needed. The `docs/` folder will hold the evidence log, and `evidence/` will hold the actual proof files.

2. Create the StateSpec files:

   ```bash
   touch AGENTS.md STATUS.md PROJECT_STATE.yaml PROJECT_DNA.yaml NEXT_ACTIONS.md BACKLOG.md WORKLOG.md docs/EVIDENCE_LOG.md
   ```

   `touch` creates empty files so the structure exists before you fill it in.

3. Turn the directory into a Git repo so the files are tracked:

   ```bash
   git init
   ```

   `git init` creates a new Git repository in the current directory so changes to the StateSpec files can be tracked.

4. Add the files to Git and check the status:

   ```bash
   git add .
   git status
   ```

   You should see the StateSpec files and directories listed as "new file" or "Changes to be committed".

5. Verify the files are really there:

   ```bash
   ls -la ~/Projects/study-project
   ls -la ~/Projects/study-project/evidence
   ```

   Both commands should list the expected files.

## Your first real workflow

Goal: use StateSpec to add a small feature to `~/Projects/study-project`.

### 1. Create the project repo

Run the commands in [Install and verify](#install-and-verify) so the files exist and Git is tracking them.

### 2. Tell the agent the rules

Write a short `AGENTS.md` so the agent knows what it may and may not do. For example:

```markdown
# Project rules

- Read STATUS.md, PROJECT_STATE.yaml, NEXT_ACTIONS.md, and BACKLOG.md at the start of each session.
- Propose a plan before changing code.
- Do not mark a task closed without evidence in evidence/ and docs/EVIDENCE_LOG.md.
- Update WORKLOG.md and docs/EVIDENCE_LOG.md as the session progresses.
- Ask before editing STATUS.md, PROJECT_STATE.yaml, or NEXT_ACTIONS.md.
```

This prevents the agent from guessing what to build or declaring work done without proof.

### 3. File a task

Open `STATUS.md` and write the current state:

```markdown
# Status

## Active tasks
- Add a command-line greeting script

## Blockers
- None

## Last validation
- None
```

Then add the task to `NEXT_ACTIONS.md`:

```markdown
# Next actions

1. Propose a design for a greeting script.
2. Implement the script.
3. Run the script and capture output as evidence.
```

Add later ideas to `BACKLOG.md`:

```markdown
# Backlog

- Add name argument handling
- Add a test file
```

### 4. Let the agent propose

Start a session with your agent. Point it at `AGENTS.md` and the current state files, then ask it to work on the active task. The agent should propose a plan before it edits any code.

Read the proposal. If it looks good, tell the agent to proceed. If it does not, update `NEXT_ACTIONS.md` with what is missing and ask again.

### 5. Validate

After the agent implements the script, ask it to prove the change works. For example:

```bash
python3 greeting.py > evidence/greeting-output.txt
```

This runs the script and saves the output. Then add a line to `docs/EVIDENCE_LOG.md`:

```markdown
# Evidence log

- `evidence/greeting-output.txt` — output of `python3 greeting.py` on 2026-07-06 (replace with today's date).
```

Log the session in `WORKLOG.md`:

```markdown
# Work log

## 2026-07-06
Added greeting.py. Captured output in evidence/greeting-output.txt. Script runs without errors.
```

### 6. Close the task

When the evidence looks good, update `STATUS.md`:

```markdown
## Active tasks
- None

## Last validation
- greeting.py runs and produces expected output
```

Clear or update `NEXT_ACTIONS.md` to point at the next task from the backlog.

That is the StateSpec loop: **create a project repo → file a task → let the agent propose → validate → close**.

## Common problems and fixes

The fixes below are based on common StateSpec practice. I have not tested each one on this machine.

| Symptom | Cause | Fix |
| --- | --- | --- |
| The agent starts changing files before proposing a plan | `AGENTS.md` does not require a plan first | Add the rule "Propose a plan before changing code" and point the agent back to `AGENTS.md`. |
| The agent says a task is done but there is no evidence | `docs/EVIDENCE_LOG.md` or `evidence/` is empty or was skipped | Reject the closure and require a test, screenshot, log, or diff in `evidence/` with an entry in `docs/EVIDENCE_LOG.md`. |
| `STATUS.md` shows tasks that were already finished | `STATUS.md` was not updated after validation | Update `STATUS.md` as soon as you accept the evidence. |
| The agent drifts to a backlog task before the active task is closed | `NEXT_ACTIONS.md` or `BACKLOG.md` is unclear | Point the agent back to `STATUS.md` and `NEXT_ACTIONS.md` at the start of the session. |
| `PROJECT_DNA.yaml` keeps changing every session | The agent treats project facts as working notes | Add a rule that only you may edit `PROJECT_DNA.yaml`, and keep DNA facts stable. |
| `git status` shows many untracked StateSpec files | You created the files but never added them | Run `git add .` and commit when a session ends. |
| Phone edits look fine but break YAML or Markdown formatting | Virtual keyboards and auto-correct change characters | Use a plain-text editor on `student-phone`, disable auto-correct, or do final edits on `studybox`. |
| The agent rewrites `PROJECT_STATE.yaml` to match its own claim | `AGENTS.md` allows it to update state files freely | Add "Ask before editing STATUS.md, PROJECT_STATE.yaml, or NEXT_ACTIONS.md" and review diffs before approving. |

## Safety notes

!!! warning "Implemented is not validated"
    The agent must prove a change with tests, runtime proof, or screenshots.

    - Keep the rule in `AGENTS.md` that the agent cannot close tasks without evidence.
    - Read every piece of evidence before you update `STATUS.md` or `NEXT_ACTIONS.md`.
    - Never let the agent rewrite StateSpec files just to match its own claim.
    - Push the repo to a remote or back it up regularly, especially before a long break.
    - Keep secrets, tokens, and passwords out of all StateSpec files and evidence screenshots.
    - Treat the agent's proposal as a proposal, not as the final decision.

## Phone vs laptop

| Task | Phone with Termux (`student-phone`) | Laptop (`studybox`) |
| --- | --- | --- |
| Reading `STATUS.md` or `NEXT_ACTIONS.md` | Good | Good |
| Checking a short evidence file | Good | Good |
| Writing long evidence files or logs | Hard | Easy |
| Running tests or scripts | Slow or impossible | Easy |
| Typing Markdown tables or YAML | Error-prone | Comfortable |
| Committing and pushing changes | Possible | Easier |

Treat the phone as a status checker. Do the main planning, validation, evidence capture, and state updates on `studybox`.

## What to read next

- Read the [StudyState guide](studydd.md) to manage learning topics the same repo-first way.
- Read the [Safety Boundaries page](../safety/boundaries.md) for rules that protect you and your data while using an AI agent.
