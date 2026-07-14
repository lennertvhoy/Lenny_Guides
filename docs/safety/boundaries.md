# Boundaries

## One-line summary

Set simple, non-negotiable limits so an AI coding agent acts like a fast junior colleague in your terminal instead of a reckless administrator.

## What it does

Boundaries tell you and the agent what is normal work and what needs a human decision. They protect three things:

- **Your repo.** The Git repo is the source of truth. Plans, evidence, and state files live inside it.
- **Your secrets.** API keys, tokens, and passwords never belong in prompts, screenshots, or commits.
- **Your machine.** The agent proposes commands, installs, and commits; you review before anything runs.

With boundaries in place, every unexpected agent request becomes a small decision tree instead of a panic moment. This page walks through three common decisions and gives you a checklist you can reuse.

## When to use it

- Before you start an agent session on `studybox`.
- When the agent asks to install a package you do not recognize.
- When the agent wants to commit a file that looks like `.env`.
- When the agent suggests a shell command you do not fully understand.
- When you switch from laptop to `student-phone` and need to decide what is safe to approve on a small screen.

## What you need

- A computer called `studybox` with a terminal and Git installed.
- A user account `lenny` on `studybox`. Replace it with your own username.
- A project repo at `~/Projects/study-project`. Replace it with your own project path.
- An Android phone called `student-phone` for the phone-vs-laptop notes.
- Basic terminal skills: running commands, reading output, and editing text files.

## Install and verify

These commands create a safety baseline inside `~/Projects/study-project`.

1. Change into your project directory:

   ```bash
   cd ~/Projects/study-project
   ```

   `cd` changes your shell's working directory to the repo.

2. Make sure `.env` and secret-like files are ignored by Git:

   ```bash
   cat .gitignore
   ```

   `cat` prints the contents of the file. Look for lines like `.env`, `.env.*`, `*.key`, `*.pem`, and `secrets/`.

3. If those lines are missing, add them:

   ```bash
   cat >> .gitignore << 'EOF'
   .env
   .env.*
   !.env.example
   *.key
   *.pem
   secrets/
   EOF
   ```

   This appends the secret-ignore rules to `.gitignore`. The `!.env.example` line means Git *does* track an example file that contains fake values.

4. Create an example environment file with no real secrets:

   ```bash
   cat > .env.example << 'EOF'
   # Copy this file to .env and fill in your own values
   API_KEY=your-key-here
   DATABASE_URL=sqlite:///dev.db
   EOF
   ```

   `.env.example` shows the agent the shape of the configuration without exposing your real credentials.

5. Verify the repo is clean and that `.env` is ignored:

   ```bash
   git status
   ```

   If `.env` is listed as untracked, add it to `.gitignore` and run `git status` again. It should no longer appear.

6. Add the example files to Git:

   ```bash
   git add .gitignore .env.example
   git status
   ```

   You should see `.gitignore` and `.env.example` ready to commit. Your real `.env` is still private.

## Your first real workflow

The best way to learn boundaries is to practice three real decisions. All of them happen while you are working in `~/Projects/study-project` on `studybox`.

### 1. The agent asks to install a package you do not recognize

The agent prints:

```text
I need to install `sentiment-cli` to analyze text. Run:

python3 -m pip install sentiment-cli
```

Stop. Do not run the command yet.

1. Ask the agent what the package does and where it comes from.
2. Preview what would be installed without changing your environment:

   ```bash
   python3 -m pip install --dry-run sentiment-cli
   ```

   `--dry-run` simulates the install and shows the package version and dependencies, but does not write anything to disk.

3. Check that the package looks legitimate. Look at the project URL, version number, and number of downloads. If anything feels off, say no.
4. If you approve, install it and verify it imports cleanly:

   ```bash
   python3 -m pip install sentiment-cli
   python3 -c "import sentiment_cli; print(sentiment_cli.__version__)"
   ```

   The second command loads the package and prints its version. If it fails, the install is broken or the package name is wrong.

5. Add the package to `requirements.txt` so the dependency is tracked in Git:

   First check whether `sentiment-cli` is already listed so you do not create a duplicate entry:

   ```bash
   grep -i "sentiment-cli" requirements.txt || python3 -m pip freeze | grep sentiment-cli >> requirements.txt
   ```

   `grep -i` searches `requirements.txt` case-insensitively. If the package is already there, the command does nothing. If it is missing, `pip freeze` lists installed packages, `grep` selects the line for `sentiment-cli`, and `>>` appends it once.

   Alternatively, open `requirements.txt` in your editor and add the line manually if it is not already present.

### 2. The agent wants to commit a file that looks like `.env`

The agent says:

```text
I have created a `.env` file with your database password. I will add it to the next commit.
```

Stop. A `.env` file usually contains secrets and must not be committed.

1. Check what Git sees:

   ```bash
   git status
   ```

   You should see `.env` as an untracked or staged file.

2. If the agent already staged it, remove it from the staging area:

   ```bash
   git restore --staged .env
   ```

   `git restore --staged` un-stages the file without deleting it from your working directory.

3. Make sure `.env` is in `.gitignore`:

   ```bash
   grep -E "^\.env" .gitignore
   ```

   This checks for any rule that starts with `.env`, such as `.env` or `.env.*`. If nothing matches, review `.gitignore` visually and add `.env` and `.env.*` as needed.

4. Create an example file that is safe to commit:

   !!! warning "Never put real secrets in `.env.example`"
       `.env.example` is meant to be committed to Git. It must contain only fake placeholder values. Never copy a real `.env` file into it.

   Create `.env.example` from scratch with placeholder values:

   ```bash
   cat > .env.example << 'EOF'
   # Copy this file to .env and fill in your own values
   API_KEY=your_api_key_here
   DATABASE_PASSWORD=your-password-here
   DATABASE_URL=sqlite:///dev.db
   EOF
   ```

   Then open `.env.example` in your editor and confirm every value is a placeholder, not a real secret.

5. Stage only the safe files:

   ```bash
   git add .gitignore .env.example
   git status
   ```

   `.env` should no longer appear in the status output. `.gitignore` and `.env.example` should be ready to commit.

### 3. The agent suggests a shell command you do not understand

The agent prints:

```text
Clean up old logs with:

find / -name "*.log" -exec rm -f {} \;
```

Stop. This command searches your entire filesystem and force-deletes every `.log` file. It could break other programs.

1. Ask the agent to explain every part of the command. In this case:
   - `find /` starts searching at the root of the filesystem.
   - `-name "*.log"` matches files ending in `.log`.
   - `-exec rm -f {} \;` deletes each matched file without asking.

2. Read the manual for the command:

   ```bash
   man find
   ```

   Press `q` to quit the manual viewer.

3. Run a safe, read-only version first, limited to your project:

   ```bash
   find ~/Projects/study-project -name "*.log" -print
   ```

   `-print` lists the matching files without deleting anything. Read the list.

4. If the list only contains logs you really want to delete, run the destructive version inside your project only:

   ```bash
   find ~/Projects/study-project -name "*.log" -delete
   ```

   `-delete` removes only the files inside `~/Projects/study-project`. The scope is narrow and the command is no longer a mystery.

5. If the command is still unclear, reject it and ask the agent for a simpler, scoped alternative.

## Common problems and fixes

The table below lists common mistakes, why they are unsafe, and the safe alternative.

| Common mistake | Why it is unsafe | Safe alternative |
|---|---|---|
| Pasting an API key into the agent chat | The key may be logged, stored, or leaked in a screenshot. | Put the key in `.env` and never mention it in chat. Use `.env.example` for shape-only examples. |
| Committing `.env` | Real secrets become permanent in Git history. | Add `.env` to `.gitignore`, commit `.env.example` instead. |
| Running `curl ... \| sh` without reading the script | You are executing code from the internet sight unseen. | Read the script first, use the official installer, or install through your package manager. |
| Using `sudo` just because the agent asked | It gives the agent root access for one command; typos can break the system. | Ask why root is needed, look up the command, and run it yourself only if you understand it. |
| Force-pushing `git push --force` | It overwrites remote history and can delete someone else's work. | Use `git push --force-with-lease` only when you are sure no one else pushed. |
| Running a shell command you do not understand | Hidden flags can delete files, exfiltrate data, or install malware. | Read the manual, run a read-only or scoped version, and reject what you cannot explain. |
| Taking a screenshot where a token is visible | Keys can be read from images by anyone you share them with. | Blur or crop the secret, or use text logs with the secret replaced by `***`. |

## Safety notes

!!! warning "The agent proposes; you decide"
    Never let an agent run destructive or irreversible actions without your explicit approval.

- **Repo first.** The Git repo in `~/Projects/study-project` is the source of truth. Plans and evidence live there, not in the chat thread.
- **Plan before build.** For any non-trivial task, the agent should explain the plan before changing files.
- **Evidence before closure.** A task is not done because the agent says so. Ask for tests, screenshots, logs, or a clean diff.
- **No secrets in chat or Git.** API keys, tokens, and passwords do not belong in prompts, screenshots, or commits.
- **One task at a time.** Parallel agents only when they do not touch the same files or decisions.
- **Scope everything.** Prefer commands that affect `~/Projects/study-project` over commands that affect the whole filesystem.
- **Human stays reviewer.** Accept, modify, or reject every proposal.

## Phone vs laptop

| Task | Laptop on `studybox` | Phone `student-phone` |
|---|---|---|
| Reading a short status message | Good | Good |
| Approving a one-line fix | Good | Good |
| Reviewing a large diff | Easy | Hard |
| Installing unknown packages | Do this on `studybox` | Avoid |
| Committing files | Easy | Error-prone |
| Security decisions such as `.env`, `sudo`, or force-push | Required | Avoid |

Use your phone for quick checks and small approvals. Do installs, large reviews, merges, and security decisions on `studybox`.

## What to read next

- [Herdr guide](../guides/herdr.md) — keep agent sessions running on `studybox` and reattach from `student-phone` safely.
- [StateSpec guide](../guides/statedd.md) — keep plans, status, and evidence inside the repo so the agent always starts from the truth.
