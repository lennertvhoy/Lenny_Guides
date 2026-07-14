# StudyState

> **Naming:** StudyState is the learning application/template. Existing
> repositories, paths, environment variables, and schemas may still use the
> legacy `StudyDD` compatibility identifier.

## One-line summary

StudyState keeps your learning topic, sources, weak points, review schedule, and proof of understanding inside the same Git repo where your agent works, so nothing is lost between sessions.

## What it does

StudyState is a repo-first learning workflow. Instead of scattering notes across chat threads, browser tabs, and notebooks, you store the important parts in plain-text files inside `~/Projects/study-project`. You and your agent both read and update the same files, so the repo always shows the current truth.

The file set is:

- `AGENTS.md` — rules for the agent in this study repo, for example "do not mark topics mastered without evidence".
- `STATUS.md` — what you are currently learning, what is mastered, and how confident you feel.
- `NEXT_ACTIONS.md` — the exact next step to take in the next study session.
- `BACKLOG.md` — topics you want to learn later.
- `WORKLOG.md` — a short log of what happened in each session.
- `SOURCES.md` — links, books, videos, or courses you used.
- `REVIEW.md` — spaced-review checklist and dates.
- `evidence/README.md` — a catalog of evidence files such as quizzes, tests, or produced artifacts.

The agent reads these files at the start of a session and writes updates only to the files you allow.

## When to use it

Use StudyState when you want the agent to help you learn something new and you need to keep track of progress over days or weeks:

- Learning a new programming language.
- Learning a new framework or library.
- Learning a new computer-science concept with an AI tutor.
- Preparing for an exam, interview, or certification.

It works best when each session ends with a small piece of evidence you can review later.

## What you need

This guide uses `lenny` as the username on `studybox` and `student-phone` as your Android device. Replace them with your own names.

- A study machine called `studybox` with Git and a terminal installed.
- A user account `lenny` on `studybox`.
- A directory for the repo at `~/Projects/study-project`.
- Your Android phone `student-phone` with Termux, if you want to review on the go.
- About ten minutes for the first setup.

## Install and verify

1. Create the repo directory and the evidence folder:

   ```bash
   mkdir -p ~/Projects/study-project/evidence
   cd ~/Projects/study-project
   ```

   `mkdir -p` creates parent directories as needed. The `evidence/` folder will hold quizzes, code samples, and other proof.

2. Create the StudyState files:

   ```bash
   touch AGENTS.md STATUS.md NEXT_ACTIONS.md BACKLOG.md WORKLOG.md SOURCES.md REVIEW.md evidence/README.md
   ```

   `touch` creates empty files so the structure exists before you fill it in.

3. Turn the directory into a Git repo so the files are tracked:

   ```bash
   git init
   ```

4. Add the files to Git and check the status:

   ```bash
   git add .
   git status
   ```

   You should see the eight StudyState files listed as "new file" or "Changes to be committed".

5. Verify the files are really there:

   ```bash
   ls -la ~/Projects/study-project
   ls -la ~/Projects/study-project/evidence
   ```

   Both commands should list the expected files.

## Your first real workflow

Goal: use StudyState to learn **Python dictionaries** in `~/Projects/study-project`.

### 1. Create the study repo

Run the commands in [Install and verify](#install-and-verify) so the files exist and Git is tracking them.

### 2. Tell the agent the rules

Write a short `AGENTS.md` so the agent knows what it may and may not do. For example:

```markdown
# Study repo rules

- Read STATUS.md, NEXT_ACTIONS.md, and BACKLOG.md at the start of each session.
- Do not mark a topic mastered unless REVIEW.md contains a passed quiz or artifact.
- Update WORKLOG.md, SOURCES.md, and evidence/README.md as the session progresses.
- Ask before editing STATUS.md or REVIEW.md.
```

This prevents the agent from guessing your progress for you.

### 3. Add a learning topic

Open `STATUS.md` and write:

```markdown
# Status

## Current focus
Python dictionaries

## Confidence
2 / 5

## Mastered
- None yet
```

Then add the next step to `NEXT_ACTIONS.md`:

```markdown
# Next actions

1. Ask the agent to explain Python dictionaries with examples.
2. Complete the mini-quiz and save it to evidence/.
```

Finally, add later topics to `BACKLOG.md`:

```markdown
# Backlog

- Python sets
- Python list comprehensions
```

### 4. Study and write evidence

Run a session with your agent. Ask it to explain dictionaries and create a short quiz. Save the result:

```bash
# Create the quiz file
nano evidence/quiz-dictionaries.md
```

Type the questions, your answers, and the agent's feedback. Then add a line to `evidence/README.md`:

```markdown
# Evidence catalog

- `quiz-dictionaries.md` — mini-quiz on Python dictionaries, passed.
```

Record the source in `SOURCES.md`:

```markdown
# Sources

- Python docs: https://docs.python.org/3/tutorial/datastructures.html#dictionaries
```

And log the session in `WORKLOG.md`:

```markdown
# Work log

## 2026-07-06
Studied Python dictionaries. Created quiz in evidence/quiz-dictionaries.md. Need review in 2 days.
```

### 5. Review

Before you mark the topic mastered, read the evidence yourself. If it is good, update `REVIEW.md`:

```markdown
# Review log

## Python dictionaries
- First review: 2026-07-06 — passed quiz.
- Next review: 2026-07-08
```

Then update `STATUS.md`:

```markdown
## Current focus
None

## Mastered
- Python dictionaries
```

Clear `NEXT_ACTIONS.md` or point it at the next backlog topic.

That is the StudyState loop: **create a study repo → tell the agent the rules → add a learning topic → write evidence → review**.

## Common problems and fixes

| Symptom | Cause | Fix |
| --- | --- | --- |
| The agent keeps overwriting `STATUS.md` without asking | `AGENTS.md` does not forbid it | Add a rule: "Ask before editing STATUS.md or REVIEW.md". |
| You cannot remember where a fact came from | `SOURCES.md` is empty or not updated | Add the link or book title before you start the topic. |
| The agent says a topic is mastered but there is no proof | `REVIEW.md` was updated by the agent instead of by you | Always review the evidence yourself before updating `REVIEW.md` or `STATUS.md`. |
| The `evidence/` folder is hard to browse | `evidence/README.md` is missing or stale | List every evidence file with a one-line description after each session. |
| You forget when to review a topic | `REVIEW.md` has no next-review date | Add a "Next review" line with a date every time you finish a review. |
| `git status` shows many untracked StudyState files | You created the files but never added them | Run `git add .` and commit when a session ends. |
| Phone edits look fine but break Markdown formatting | Virtual keyboards and auto-correct change characters | Use a plain-text editor on `student-phone`, disable auto-correct, or do final edits on `studybox` (unverified). |
| The agent drifts to a different topic | `NEXT_ACTIONS.md` or `BACKLOG.md` is unclear | Point the agent back to `STATUS.md` and `NEXT_ACTIONS.md` at the start of the session (unverified). |

## Safety notes

!!! warning "Mastery needs evidence"
    Do not mark a topic mastered without a test, quiz, or produced artifact.

- Keep the rule in `AGENTS.md` that the agent cannot mark topics mastered.
- Read every piece of evidence before you update `REVIEW.md` or `STATUS.md`.
- Push the repo to a remote or back it up regularly, especially before a long break.
- Be honest about weak areas; `STATUS.md` confidence scores are for you, not the agent.
- If a link in `SOURCES.md` no longer works, note it so you do not rely on a dead source.

## Phone vs laptop

| Task | Phone with Termux (`student-phone`) | Laptop (`studybox`) |
| --- | --- | --- |
| Reading `STATUS.md` or `NEXT_ACTIONS.md` | Good | Good |
| Quick review of flashcards or quiz answers | Good | Good |
| Writing long evidence files | Hard | Easy |
| Running code examples or tests | Slow or impossible | Easy |
| Typing Markdown tables | Error-prone | Comfortable |
| Committing and pushing changes | Possible | Easier |

Treat the phone as a review companion. Do the main learning, evidence writing, and final review on `studybox`.

## What to read next

- Read the [StateSpec guide](statedd.md) to manage whole projects the same repo-first way.
- Read the [Safety Boundaries page](../safety/boundaries.md) for rules that protect you and your data while using an AI agent.
