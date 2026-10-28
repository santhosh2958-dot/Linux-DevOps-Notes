# 🌿 Git & GitHub — Complete Beginner-to-Advanced DevOps Guide

> **Repository path:** `Linux-DevOps-Notes/05-Git-GitHub/README.md`  
> **Audience:** Absolute beginners through advanced DevOps learners  
> **Practice:** Windows PowerShell / VS Code, Ubuntu Linux, GitHub, GitHub Actions  
> **Goal:** Understand each command, practice safely, troubleshoot confidently, and explain Git in interviews.

---

## 📚 Table of Contents

1. [Learning roadmap](#1-learning-roadmap)
2. [Git vs GitHub, why DevOps needs both](#2-git-vs-github-why-devops-needs-both)
3. [Core Git mental model and diagrams](#3-core-git-mental-model-and-diagrams)
4. [Install Git on Windows and Ubuntu](#4-install-git-on-windows-and-ubuntu)
5. [First-time configuration and identity](#5-first-time-configuration-and-identity)
6. [Repository creation, cloning, and structure](#6-repository-creation-cloning-and-structure)
7. [Everyday commands with examples](#7-everyday-commands-with-examples)
8. [Status, diffs, history, and inspection](#8-status-diffs-history-and-inspection)
9. [Branches, HEAD, and switching](#9-branches-head-and-switching)
10. [Merging and conflict resolution](#10-merging-and-conflict-resolution)
11. [Remote repositories and synchronization](#11-remote-repositories-and-synchronization)
12. [GitHub authentication: HTTPS, SSH, and tokens](#12-github-authentication-https-ssh-and-tokens)
13. [Undo, restore, reset, revert, and reflog](#13-undo-restore-reset-revert-and-reflog)
14. [Stash, tags, cherry-pick, and rebase](#14-stash-tags-cherry-pick-and-rebase)
15. [.gitignore, .gitattributes, and line endings](#15-gitignore-gitattributes-and-line-endings)
16. [GitHub collaboration and pull requests](#16-github-collaboration-and-pull-requests)
17. [Advanced Git and repository maintenance](#17-advanced-git-and-repository-maintenance)
18. [GitHub Actions and CI/CD](#18-github-actions-and-cicd)
19. [Git security and DevOps best practices](#19-git-security-and-devops-best-practices)
20. [Mini project: AWS learning notes repository](#20-mini-project-aws-learning-notes-repository)
21. [Intermediate project: feature branch and pull request](#21-intermediate-project-feature-branch-and-pull-request)
22. [Advanced project: Python CI/CD with GitHub Actions](#22-advanced-project-python-cicd-with-github-actions)
23. [Troubleshooting real errors](#23-troubleshooting-real-errors)
24. [Interview questions and answers](#24-interview-questions-and-answers)
25. [Command reference and revision checklist](#25-command-reference-and-revision-checklist)
26. [Official documentation](#26-official-documentation)

---

## 1. Learning roadmap

| Level | Learn | Demonstrate mastery by |
|---|---|---|
| Beginner | Git/GitHub, init, clone, status, add, commit, push | Publish your own README |
| Foundation | diff, log, ignore, branches, pull, fetch | Maintain a notes repository daily |
| Intermediate | merge, conflicts, pull requests, tags, stash | Complete a feature-branch review |
| Advanced | rebase, cherry-pick, reflog, reset, bisect, worktree | Recover a mistake and trace a regression |
| DevOps | GitHub Actions, branch rules, secrets, deployment controls | Build a tested CI workflow |

**Study rule:** Read the explanation, type the command yourself, predict its output, run it, then write down what changed. Do not memorize commands without understanding their effects.

## 2. Git vs GitHub, why DevOps needs both

**Git** is distributed version-control software. It records snapshots (commits), lets you compare changes, create branches, merge work, and recover history. A local Git repository works without internet.

**GitHub** hosts Git repositories and adds collaboration: pull requests, issues, reviews, permissions, Actions automation, releases, and security features. Other Git hosts include GitLab and Bitbucket.

| Question | Git | GitHub |
|---|---|---|
| What is it? | Version-control software | Online Git hosting and collaboration platform |
| Requires internet? | No for local operations | Yes to access the hosted service |
| Saves history? | Locally | Stores pushed Git history remotely |
| Example | `git commit` | Open a pull request |

**DevOps example:** A developer changes an application's code, commits it, opens a pull request, tests run automatically, reviewers approve, and a deployment pipeline releases the approved version. Git tracks the changes; GitHub coordinates and automates the workflow.

**Important:** A Git repository is not automatically a backup of uncommitted work. GitHub only receives commits you push.

## 3. Core Git mental model and diagrams

```text
                    YOUR COMPUTER                            ONLINE

Working directory       Staging area          Local Git repo       GitHub repo
(edit files)            (index)               (commits)            (remote)
     |                       |                     |                   |
     +---- git add ---------->                     |                   |
                             +---- git commit ---->                   |
                                                   +--- git push ----->|
                                                   <--- git fetch -----+

          git status: see changes       git log: see saved history
```

**Working directory:** Your visible project files. **Staging area/index:** Exact changes selected for the next commit. **Commit:** An immutable snapshot identified by a hash and containing metadata. **Branch:** A movable name pointing to a commit. **HEAD:** Usually the currently checked-out branch. **Remote:** A named connection to another Git repository, commonly `origin`.

```text
A---B---C  main
     \
      D---E  feature/webserver
```

The branch names point at different commits. You can work on `feature/webserver` without changing `main` until you merge.

### File states

- **Untracked:** Git has not been told to track this new file.
- **Modified:** A tracked file differs from the staged or committed version.
- **Staged:** Selected content is ready for the next commit.
- **Committed:** Saved into Git history.
- **Pushed:** Relevant local commits have been uploaded to a remote branch.

## 4. Install Git on Windows and Ubuntu

### Windows

Install **Git for Windows** from https://git-scm.com/downloads/win, then open PowerShell or the VS Code terminal:

```powershell
git --version
where.exe git
```

VS Code: **Terminal → New Terminal**. Git Bash is another available shell; PowerShell commands and Bash commands differ for file creation and directory listing.

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install -y git
git --version
which git
```

### RHEL / Amazon Linux (depending on release)

```bash
sudo dnf install -y git
git --version
```

Check `git --help`, `git help commit`, or `git commit -h` for command help.

## 5. First-time configuration and identity

```bash
git config --global user.name "Your Name"
git config --global user.email "your-verified-email@example.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --global --list
git config --show-origin --get user.email
```

Replace placeholders. `--global` applies to your user; `--local` (the default inside a repository) applies only there; `--system` applies system-wide and usually needs administrative access. Repository settings generally override global settings.

**Privacy:** Commit author emails may be visible to repository readers. Use a verified personal email or GitHub no-reply email, not an automatically detected company address for personal notes. Git author identity is not the same as GitHub authentication.

```bash
# Only for this repository:
git config user.name "Your Name"
git config user.email "your-verified-email@example.com"
```

### Configuration and line endings

Windows often uses CRLF, Linux uses LF. See section 15 for `.gitattributes`. LF-to-CRLF notices are usually warnings, not failed commits.

## 6. Repository creation, cloning, and structure

**Repository** = project files plus hidden `.git/` history directory.

### Create a new local repository

```bash
mkdir git-practice
cd git-practice
git init -b main
git status
```

### Clone an existing GitHub repository

```bash
git clone https://github.com/santhosh2958-dot/Linux-DevOps-Notes.git
cd Linux-DevOps-Notes
git remote -v
git branch -a
```

`clone` downloads the Git repository and checks out a working copy. Do **not** run `git init` again inside a cloned repository.

### Connect an existing local repository

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git branch -M main
git push -u origin main
```

Replace the URL. The remote GitHub repository should already exist; if it contains unrelated commits, follow the safe synchronization procedure rather than forcing a push.

### What is inside `.git/`?

- `objects/`: commit, tree, blob, and tag objects.
- `refs/`: branch and tag references.
- `HEAD`: current branch or commit reference.
- `config`: local repository configuration.
- `index`: staging-area representation.

Do not manually edit `.git` unless you are deliberately doing advanced recovery.

## 7. Everyday commands with examples

| Command | Meaning | Example / important options |
|---|---|---|
| `git init` | Start repository | `git init -b main` |
| `git clone` | Copy remote repository | `git clone URL`; `--depth 1` shallow clone |
| `git status` | Show file/branch status | `git status -sb` concise output |
| `git add` | Stage changes | `git add README.md`, `git add .`, `git add -p` |
| `git commit` | Save staged snapshot | `git commit -m "Document EC2"` |
| `git push` | Upload commits | `git push -u origin main` first time; `git push` later |
| `git pull` | Fetch and integrate | `git pull --ff-only` if fast-forward possible |
| `git fetch` | Download remote history | `git fetch origin` without merging |
| `git rm` | Remove tracked file | `git rm old.md` then commit |
| `git mv` | Rename tracked file | `git mv old.md new.md` |

### Your daily cycle

```bash
git status
git diff                 # What changed but is not staged?
git add README.md        # Select just the file you want
git diff --cached        # Review staged changes
git commit -m "Add IAM interview questions"
git push
git status
```

**Expected clean state:** `nothing to commit, working tree clean`. A clean working tree does not itself prove GitHub is synchronized; inspect `git status -sb` and tracking status.

### Useful `git add` options

```bash
git add .                # Stage changes under current directory
git add -A               # Stage changes across entire repository
git add -u               # Stage tracked-file modifications/deletions
git add -p               # Interactively choose portions (hunks)
git add -- README.md     # -- separates options from pathnames
```

### Useful `git commit` options

```bash
git commit -m "Add S3 notes"
git commit -am "Fix existing notes"  # Tracked files only; new files NOT included
git commit --amend                  # Replace most recent local commit
```

**Warning:** Do not amend a commit that teammates already pulled unless you understand history rewriting and have coordinated it.

## 8. Status, diffs, history, and inspection

```bash
git status
git status --short
git diff                         # Working tree vs index
git diff --staged                # Index vs last commit
git diff HEAD                   # Working tree vs HEAD
git diff main..feature/docs     # Compare two branch tips
git log
git log --oneline --graph --decorate --all
git log -5 -- README.md         # Last five commits affecting a file
git show HEAD                   # Latest commit details
git show HEAD:README.md         # README content at HEAD
git blame README.md             # Line-by-line last change attribution
git ls-files                    # Files tracked by Git
git rev-parse --show-toplevel   # Repository root
git rev-parse --short HEAD      # Current commit short ID
```

**`HEAD~1`** means the first parent of the current commit; **`HEAD^`** also means its first parent. A commit hash identifies a snapshot, not a filename.

**When useful:** Before committing, inspect `diff`; during troubleshooting, inspect `log`, `show`, and `blame` to understand what changed and why.

## 9. Branches, HEAD, and switching

A branch allows independent work without immediately changing the mainline.

```bash
git branch                         # Local branches
git branch -a                      # Local + remote-tracking branches
git switch -c feature/ec2-guide    # Create and switch
git switch main                    # Switch back
git switch -                       # Previous branch
git branch -m feature/new-name     # Rename current branch
git branch -d feature/ec2-guide    # Delete safely if merged
git branch -D feature/ec2-guide    # Force-delete local branch: use carefully
```

`git checkout -b feature/name` is older syntax for creating/switching. `git switch` is clearer for branch operations; `git restore` is clearer for restoring file content.

### Branch visualization

```text
main:          A---B---C
                   \
feature/docs:       D---E
```

After merging the feature, `main` includes its changes. The exact history depends on whether the merge is fast-forward, merge commit, squash, or rebase-based.

### Detached HEAD

```bash
git switch --detach HEAD~1
```

You are viewing a specific commit rather than a branch. New commits can become hard to find if you leave without creating a branch. To preserve experiments:

```bash
git switch -c experiment/old-version
```

## 10. Merging and conflict resolution

**Merge** integrates changes from another branch. Always inspect your status and make sure you are on the intended target branch.

```bash
git switch main
git merge feature/docs
git log --oneline --graph --all
```

**Fast-forward merge:** Main has not diverged, so its pointer moves forward. **Three-way merge:** Git combines divergent histories and may create a merge commit.

### Conflict example

Two branches edit the same line of `README.md` differently. Git may show:

```text
<<<<<<< HEAD
AWS EC2 tutorial
=======
AWS EC2 and S3 tutorial
>>>>>>> feature/docs
```

**Resolve step by step:**

1. Run `git status` to identify conflicted files.
2. Open the file and decide what final text should be.
3. Remove all conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
4. Stage resolved files with `git add README.md`.
5. Complete with `git commit` if Git requests it, or use `git merge --continue`.
6. Run tests, then `git push`.

```bash
git diff --check       # Detect whitespace errors/conflict-marker-like issues
git merge --abort      # Return to pre-merge state when possible
```

**Do not** blindly select “Accept Current” or “Accept Incoming”; inspect the intended final result.

## 11. Remote repositories and synchronization

**Remote** is an alias for another repository URL. `origin` is a convention, not a Git keyword. `origin/main` is a *remote-tracking reference*, updated by fetch/push operations; it is not the GitHub branch itself.

```bash
git remote -v
git remote add origin https://github.com/USERNAME/REPO.git
git remote set-url origin https://github.com/USERNAME/NEW-REPO.git
git remote rename origin upstream
git fetch origin
git branch -vv
git push -u origin main
git push origin feature/docs
git push origin --delete feature/docs
```

### Fetch vs pull vs push

| Command | What it does | Typical use |
|---|---|---|
| `fetch` | Downloads remote history/refs without integrating into current branch | Inspect incoming changes safely |
| `pull` | Fetches and then integrates (merge/rebase, per options/config) | Update your local branch |
| `push` | Uploads commits and updates remote branch if permitted | Publish your work |

**Safer update pattern:**

```bash
git status
git fetch origin
git log --oneline HEAD..origin/main
git switch main
git pull --ff-only origin main
```

If your branch diverged, choose an intentional merge or rebase instead of forcing the update. `git pull --rebase` replays local commits on top of remote changes and rewrites local commit IDs; use it only when appropriate.

## 12. GitHub authentication: HTTPS, SSH, and tokens

**Git author identity** (`user.name`, `user.email`) identifies commits. **Authentication** proves you have permission to access GitHub.

### HTTPS (recommended for many Windows beginners)

```bash
git remote -v
git push
```

Git Credential Manager may open a browser sign-in. GitHub does **not** accept your account password as a Git HTTPS password. Use a supported credential manager, GitHub CLI, or a suitably scoped personal access token when required.

### GitHub CLI

```bash
gh auth login
gh auth status
```

Install GitHub CLI separately if `gh` is not recognized. Follow the interactive login instructions.

### SSH authentication

```bash
ssh-keygen -t ed25519 -C "your-verified-email@example.com"
```

Accept a secure path and use a passphrase. Add the **public** key (`.pub`) to GitHub → Settings → SSH and GPG keys. Never upload your private key.

```bash
ssh -T git@github.com
git remote set-url origin git@github.com:USERNAME/REPO.git
```

For corporate computers, follow employer policies before creating keys or installing tools.

### Token security

A personal access token is sensitive like a password. Never paste it into a README, commit it, or print it in logs. Prefer fine-grained, limited-duration, least-privilege credentials where suitable.

## 13. Undo, restore, reset, revert, and reflog

**Read carefully: some undo commands permanently discard uncommitted work.**

| Situation | Recommended command | Effect |
|---|---|---|
| Staged a file accidentally | `git restore --staged README.md` | Unstage; keeps working changes |
| Want to discard unstaged tracked-file edits | `git restore README.md` | **Discards** those edits |
| Want to undo a public commit | `git revert COMMIT_HASH` | Adds new reversing commit; preserves history |
| Need to adjust last unpublished commit | `git commit --amend` | Replaces latest commit |
| Want to move branch back locally | `git reset --soft HEAD~1` | Removes latest commit from branch; keeps changes staged |
| Want to uncommit and unstage locally | `git reset --mixed HEAD~1` | Keeps working changes, unstaged |
| Want to erase commit and working changes | `git reset --hard HEAD~1` | **Destructive**; avoid unless certain |

### Examples

```bash
git restore --staged README.md
git restore README.md              # WARNING: discards unstaged edits
git revert abc1234                # Safe for shared history; creates new commit
git reflog                        # Local record of recent HEAD/ref movements
```

**Recovery scenario:** You accidentally reset a local branch. Run `git reflog`, identify the prior commit, then create a recovery branch before trying risky changes:

```bash
git reflog
git branch recovery/pre-reset <commit-hash>
git switch recovery/pre-reset
```

Reflog is local and entries expire; it is not a permanent remote backup. Avoid `git push --force` on shared branches. If a coordinated rewrite is essential, `--force-with-lease` is safer than unconditional force, but still rewrites remote history.

## 14. Stash, tags, cherry-pick, and rebase

### Stash: temporarily shelve uncommitted work

```bash
git stash push -m "WIP EC2 notes"
git stash list
git stash show -p stash@{0}
git stash apply stash@{0}      # Restore but keep stash
git stash pop                 # Restore and remove on success
git stash push -u -m "Include untracked files"
```

PowerShell may need the stash reference quoted, e.g. `git stash show -p 'stash@{0}'`.

### Tags: mark releases

```bash
git tag -a v1.0.0 -m "First learning milestone"
git tag --list
git show v1.0.0
git push origin v1.0.0
```

### Cherry-pick: apply one commit elsewhere

```bash
git switch main
git cherry-pick abc1234
```

Creates a new commit with the selected changes on the current branch. Conflicts may need resolution; `git cherry-pick --abort` cancels an in-progress operation.

### Rebase: replay commits onto another base

```bash
git switch feature/docs
git fetch origin
git rebase origin/main
```

**Merge preserves branch topology; rebase rewrites commit identities to create a linear history.** Avoid rebasing shared commits others depend on. Resolve conflicts, `git add` files, and `git rebase --continue`; or `git rebase --abort`.

### Interactive rebase (advanced)

```bash
git rebase -i HEAD~3
```

Allows rewording, squashing, or reordering recent commits. Practice only on an unpublished practice branch first.

## 15. .gitignore, .gitattributes, and line endings

`.gitignore` prevents **untracked** matching files from being added normally; it does **not** remove already tracked secrets or files.

Example `.gitignore`:

```gitignore
# Python
__pycache__/
*.pyc
.venv/

# Secrets and local config
.env
.env.*
!.env.example
*.pem
*.key

# Editor/OS
.vscode/
.DS_Store
Thumbs.db

# Terraform local/state artifacts
.terraform/
*.tfstate
*.tfstate.*
```

Keep `.terraform.lock.hcl` tracked for normal Terraform projects; do not ignore it indiscriminately. If a sensitive file was already committed, removing it from current tracking does **not** remove it from old history. Revoke/rotate exposed credentials and follow a history-remediation procedure.

```bash
git check-ignore -v .env
git rm --cached .env       # Stop tracking without deleting local file
git add .gitignore
git commit -m "Ignore local secrets"
```

Recommended `.gitattributes`:

```gitattributes
* text=auto
*.md text eol=lf
*.sh text eol=lf
*.yml text eol=lf
*.yaml text eol=lf
*.ps1 text eol=crlf
```

Windows may use CRLF in the working tree; Bash scripts should use LF. After changing line-ending rules, test carefully and review any normalization changes before committing.

## 16. GitHub collaboration and pull requests

A **pull request (PR)** proposes changes from one branch into another, supports review, and can trigger automated checks.

### Standard team workflow

1. Create an issue describing the change and acceptance criteria.
2. Update your local `main`: `git switch main` then `git pull --ff-only`.
3. Create a feature branch: `git switch -c feature/iam-guide`.
4. Edit, test, stage, commit.
5. Push: `git push -u origin feature/iam-guide`.
6. On GitHub, select **Compare & pull request**.
7. Explain *what changed, why, how tested, and risks*.
8. Address review comments and ensure checks pass.
9. Merge using the repository's approved strategy.
10. Sync local `main` and delete the completed feature branch.

### Example PR description

```markdown
## Summary
- Added IAM users vs roles comparison
- Added least-privilege hands-on example

## Testing
- [x] Markdown renders correctly
- [x] Links checked
- [x] No credentials committed

## Risk
Documentation-only change
```

### Fork vs clone

**Clone** copies a repository locally. **Fork** creates your own GitHub-hosted copy of someone else's repository, often used when you lack write access to the original. A fork is commonly cloned, edited on a branch, and used to open a PR back upstream.

### Branch protection / rulesets

Teams may require PRs, passing status checks, approvals, signed commits, and restricted direct pushes. Availability and enforcement depend on repository settings and GitHub plan; consult the current GitHub documentation.

## 17. Advanced Git and repository maintenance

| Command | Purpose | Example |
|---|---|---|
| `git bisect` | Binary search for regression-causing commit | `git bisect start`, `git bisect bad`, `git bisect good <hash>` |
| `git worktree` | Work on multiple branches in separate directories | `git worktree add ../repo-hotfix -b hotfix/login main` |
| `git grep` | Search tracked content | `git grep -n 'TODO'` |
| `git clean` | Remove untracked files | `git clean -nd` **preview**; `git clean -fd` **deletes** |
| `git fsck` | Check object connectivity/integrity | `git fsck --full` |
| `git gc` | Garbage collection/optimization | `git gc` |
| `git archive` | Export tracked tree without `.git` | `git archive --format=zip HEAD -o release.zip` |
| `git submodule` | Link another repo at a pinned commit | `git submodule add URL path` |
| `git sparse-checkout` | Check out selected paths | `git sparse-checkout init --cone` |
| `git bundle` | Portable repository history archive | `git bundle create backup.bundle --all` |
| `git log -S` | Find commits adding/removing text | `git log -S 'API_URL' --oneline` |

### Bisect example

```bash
git bisect start
git bisect bad HEAD
git bisect good <known-good-commit>
# Run your test on each selected revision, mark good/bad:
git bisect good
git bisect bad
git bisect reset
```

### Git internals, simplified

Git objects include **blobs** (file contents), **trees** (directory snapshots), **commits** (snapshot plus parent and metadata), and **annotated tags**. Git hashes content to identify objects. `git cat-file -t HEAD` identifies the object type; `git cat-file -p HEAD` inspects the commit object.

### Release strategy

Use semantic-style version tags such as `v1.2.0`, release notes, and reproducible CI artifacts. Avoid using a mutable branch name as the only reference for a production release; record the commit SHA or immutable artifact digest.

## 18. GitHub Actions and CI/CD

**CI (Continuous Integration)** automatically validates changes. **CD (Continuous Delivery/Deployment)** packages or deploys approved changes according to your process.

```text
Developer -> Git commit -> Push / PR -> GitHub Actions
                                        |
                               Checkout -> Lint -> Test
                                        |
                                Build -> Artifact
                                        |
                              Approval -> Deployment
```

A GitHub Actions **workflow** is a YAML file in `.github/workflows/`. It defines triggers (`on`), jobs, runners, steps, permissions, and optional environments/secrets.

### Basic Markdown validation workflow

Create `.github/workflows/docs-check.yml`:

```yaml
name: Docs Check
on:
  push:
  pull_request:
permissions:
  contents: read
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Check whitespace
        run: git diff --check HEAD^ HEAD || true
      - name: Confirm README exists
        run: test -f README.md
```

**Note:** This example is intentionally simple. `|| true` makes the whitespace step informational rather than a failing quality gate; remove it and use a reliable comparison range if you want enforcement. For production workflows, pin third-party actions to reviewed immutable commit SHAs, set least-privilege `permissions`, and avoid executing untrusted PR content with write credentials.

### Important terms

- **Runner:** Machine executing jobs (for example, hosted Ubuntu runner).
- **Job:** Group of steps executed on a runner.
- **Step:** Command or reusable action.
- **Artifact:** Output retained from a workflow run.
- **Secret:** Sensitive configuration managed through GitHub settings.
- **Environment:** Deployment target with optional approval/protection rules.
- **OIDC:** Federated short-lived cloud authentication, preferred over long-lived AWS keys when configured correctly.

## 19. Git security and DevOps best practices

1. Never commit passwords, AWS access keys, SSH private keys, `.env` secrets, or customer data.
2. Use GitHub secret scanning and push protection where available.
3. Treat CI logs and artifacts as possible secret-exposure channels.
4. Give workflow tokens minimal permissions; use OIDC for AWS where possible.
5. Require review and checks on important branches where feasible.
6. Keep dependencies and actions reviewed and updated.
7. Use descriptive commit messages: `docs: explain EC2 security groups`.
8. Make small, reviewable commits and pull requests.
9. Never force-push a shared main branch as a routine fix.
10. Remember: GitHub is not a replacement for full backups, access controls, or disaster recovery.

### Conventional commit examples

```text
feat: add EC2 setup script
fix: correct IAM policy example
docs: explain Git merge conflicts
test: add backup script tests
chore: update dependencies
```

## 20. Mini project: AWS learning notes repository

**Goal:** Create a local notes repository, make three commits, and publish it to GitHub. **Time:** 30–60 minutes. **Prerequisite:** Git installed; GitHub account; empty remote repository if pushing.

### Windows PowerShell lab

```powershell
mkdir aws-git-lab
cd aws-git-lab
git init -b main
"# AWS Git Lab" | Set-Content README.md
mkdir EC2
"# EC2 Basics" | Set-Content EC2/README.md
git status
git add .
git commit -m "docs: initialize AWS notes"
"EC2 provides virtual servers." | Add-Content EC2/README.md
git diff
git add EC2/README.md
git commit -m "docs: explain EC2"
mkdir S3
"# S3 Basics" | Set-Content S3/README.md
git add .
git commit -m "docs: add S3 section"
git log --oneline --graph
```

Create an **empty** GitHub repository named `aws-git-lab` (do not initialize a README on GitHub), then:

```powershell
git remote add origin https://github.com/USERNAME/aws-git-lab.git
git push -u origin main
git status
```

**Expected result:** Three commits appear on GitHub, and `README.md`, `EC2/README.md`, `S3/README.md` exist. Change `USERNAME` to your username. Avoid running `git init` in an already cloned or initialized project.

### Ubuntu equivalent file creation

```bash
printf '# AWS Git Lab\n' > README.md
mkdir -p EC2
printf '# EC2 Basics\n' > EC2/README.md
```

### Lab challenges

- Add `IAM/README.md`, commit, and push.
- Edit a tracked file and observe `git diff` before staging.
- Stage a file, then unstage it with `git restore --staged`.
- Create `.gitignore` and verify a dummy `.env` file is ignored.
- Explain why `git commit` does not upload to GitHub.

## 21. Intermediate project: feature branch and pull request

**Scenario:** You are adding an AWS VPC chapter and a teammate wants to review it before merging.

```bash
git switch main
git pull --ff-only
git switch -c feature/vpc-notes
mkdir -p VPC
printf '# AWS VPC\nA VPC is a logically isolated network.\n' > VPC/README.md
git add VPC/README.md
git commit -m "docs: add VPC introduction"
git push -u origin feature/vpc-notes
```

Open GitHub, create a PR into `main`, review the diff, and merge. Then:

```bash
git switch main
git pull --ff-only
git branch -d feature/vpc-notes
```

**Conflict exercise:** Create two branches from the same starting commit, edit the same line differently, merge one, then merge the other. Resolve markers and verify the final text. Perform this in a throwaway repository.

**Acceptance criteria:** Feature branch exists, PR explains changes, diff is reviewed, merge is complete, and local main contains the VPC notes.

## 22. Advanced project: Python CI/CD with GitHub Actions

**Goal:** Build a small Python utility with automated tests and a GitHub Actions workflow. This is a CI project; production deployment is an optional extension.

### Project structure

```text
aws-inventory-ci/
├── README.md
├── inventory.py
├── test_inventory.py
├── requirements-dev.txt
├── .gitignore
└── .github/
    └── workflows/
        └── ci.yml
```

### `inventory.py`

```python
def summarize_instances(instances):
    """Count EC2-like instance records by state (no AWS credentials needed)."""
    counts = {}
    for instance in instances:
        state = instance.get("state", "unknown")
        counts[state] = counts.get(state, 0) + 1
    return counts


if __name__ == "__main__":
    demo = [{"state": "running"}, {"state": "stopped"}, {"state": "running"}]
    print(summarize_instances(demo))
```

### `test_inventory.py`

```python
from inventory import summarize_instances


def test_instance_states():
    records = [{"state": "running"}, {"state": "stopped"}, {"state": "running"}]
    assert summarize_instances(records) == {"running": 2, "stopped": 1}


def test_empty_inventory():
    assert summarize_instances([]) == {}


def test_missing_state():
    assert summarize_instances([{}]) == {"unknown": 1}
```

### `requirements-dev.txt`

```text
pytest>=8,<10
```

### `.gitignore`

```gitignore
.venv/
__pycache__/
.pytest_cache/
*.pyc
.env
```

### `.github/workflows/ci.yml`

```yaml
name: Python CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'
          cache: pip
          cache-dependency-path: requirements-dev.txt
      - name: Install test dependencies
        run: python -m pip install -r requirements-dev.txt
      - name: Run tests
        run: python -m pytest -q
```

**Version note:** Action tags are readable learning examples. For security-sensitive production repositories, replace action tags with verified full commit SHAs and review updates. Use a dependency lockfile or constraints for stronger reproducibility.

### Local verification

```bash
python -m venv .venv
# Ubuntu: source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -r requirements-dev.txt
python -m pytest -q
```

**Expected result:** `3 passed` (exact output depends on installed pytest version).

### Git workflow

```bash
git init -b main
git add .
git commit -m "ci: add tested instance inventory utility"
# Create an empty GitHub repo, then connect it:
git remote add origin https://github.com/USERNAME/aws-inventory-ci.git
git push -u origin main
```

Open the **Actions** tab and inspect the run. Then create a branch, intentionally break a test, push, and see the PR check fail. Fix it and confirm the check passes before merging.

### Advanced extensions

1. Add Ruff linting and format checks.
2. Add `pytest-cov` coverage reporting.
3. Add dependency vulnerability scanning and code scanning.
4. Add a Dockerfile, build/test a container, and publish an image to a trusted registry.
5. Add protected environments and required reviewers for deployments.
6. Use GitHub Actions OIDC + AWS IAM role to deploy without storing long-lived AWS keys.
7. Add staging deployment, smoke tests, manual production approval, and rollback strategy.
8. Use Terraform to provision an S3 bucket or test infrastructure, and run `terraform fmt -check` and `terraform validate` in CI.

**AWS safety:** OIDC requires configuring an AWS IAM identity provider and narrowly scoped role trust policy. Cloud resources may incur costs; use a sandbox and clean up after experiments. Never publish real AWS credentials.

## 23. Troubleshooting real errors

| Symptom | Likely cause | Safe checks / solution |
|---|---|---|
| `fatal: not a git repository` | Terminal is outside repository | `pwd`/`Get-Location`, `git rev-parse --show-toplevel`; `cd` into correct folder |
| `nothing to commit` | No staged changes or files ignored | `git status`, `git diff`, `git check-ignore -v FILE` |
| `Author identity unknown` | Git name/email unset | Configure `user.name` and `user.email` |
| `remote origin already exists` | Origin was already configured | `git remote -v`; use `git remote set-url origin URL` if needed |
| `src refspec main does not match any` | No commit yet or branch has another name | `git log -1`, `git branch`; commit and verify branch |
| `non-fast-forward` / `rejected` | Remote has commits not in local branch | `git fetch`, inspect history, merge/rebase appropriately; do not force blindly |
| `Permission denied (publickey)` | SSH key missing, unregistered, or unauthorized | `ssh -T git@github.com`, inspect agent/public-key setup |
| Authentication failed over HTTPS | Expired credentials or unsupported password | Reauthenticate with credential manager/CLI/token |
| Merge conflict | Two histories changed same content | `git status`, edit conflict markers, `git add`, finish merge |
| LF/CRLF warning | Different line-ending conventions | Add `.gitattributes`, inspect files, normalize carefully |
| `pathspec did not match` | Wrong path or branch | `git status`, `git branch -a`, `git ls-files` |
| File not on GitHub | Not committed/pushed, ignored, or wrong repository | `git status`, `git log -1`, `git remote -v`, inspect GitHub branch |
| Push denied by repository rules | Protected branch / required PR | Create feature branch and PR; satisfy checks |
| Secret accidentally committed | Credential in Git history | Immediately revoke/rotate credential; follow history-removal guidance |
| Detached HEAD | Checked out commit, not branch | `git status`, create branch to preserve work |
| `git pull` says branches diverged | Need explicit merge/rebase policy | Fetch and inspect, choose integration method intentionally |

### Diagnostic checklist

```bash
git --version
git status -sb
git remote -v
git branch -avv
git log --oneline --graph --decorate -10
git config --show-origin --get user.email
```

### Real case: nested repository folder

If GitHub shows `AWS-Complete-Learning-Notes/README.md` instead of a root `README.md`, the files may have been extracted inside an extra folder. Inspect the structure before moving anything:

```powershell
Get-ChildItem -Name
git rev-parse --show-toplevel
git ls-files | Select-Object -First 10
```

Do not run a second `git init` inside the nested directory. Fix the layout deliberately and commit the moves.

## 24. Interview questions and answers

### Beginner (1–20)

**1. What is Git?** A distributed version-control system for tracking file changes and coordinating development.

**2. What is GitHub?** A hosted Git collaboration platform with PRs, reviews, issues, Actions, and access controls.

**3. Git vs GitHub?** Git manages history; GitHub hosts repositories and collaboration workflows.

**4. What is a repository?** A project directory with Git metadata and tracked history.

**5. What does `git init` do?** Initializes Git metadata in the current directory.

**6. What does `git clone` do?** Downloads a repository and creates a local working copy.

**7. What is `git status`?** Shows branch, staged, unstaged, and untracked changes.

**8. What is staging?** Selecting exact changes for the next commit.

**9. What does `git add .` do?** Stages changes under the current directory, respecting ignore rules.

**10. What is a commit?** A saved snapshot with metadata, parents, and a content-derived ID.

**11. What is `git push`?** Sends local objects and updates remote references if allowed.

**12. What is `git pull`?** Fetch plus integration into the current branch.

**13. What is `git fetch`?** Downloads remote history without merging into your working branch.

**14. What is `origin`?** A conventional alias for a remote repository.

**15. What is `main`?** A conventional primary branch name; repositories may choose another.

**16. What is `HEAD`?** A reference to the current branch or checked-out commit.

**17. What is `.gitignore`?** Patterns controlling which untracked files Git ignores.

**18. What is `.git`?** The internal metadata and object store of a Git repository.

**19. Why use commit messages?** To explain why a change was made and help review/debug history.

**20. Does committing upload files?** No. A separate push is normally needed to upload to GitHub.

### Intermediate (21–40)

**21. What is a branch?** A movable reference to a commit, used for isolated lines of development.

**22. How do you create a branch?** `git switch -c feature/name`.

**23. What is merging?** Integrating changes from another line of history.

**24. What is a merge conflict?** Git cannot automatically combine competing changes and requires a decision.

**25. How do you resolve conflicts?** Inspect files, edit final content, remove markers, stage, and finish the operation.

**26. What is a fast-forward merge?** Moving a branch pointer forward when no divergent target commits exist.

**27. What is a pull request?** A proposal to integrate changes, with review and automated checks.

**28. Fork vs clone?** Fork makes a hosted copy; clone downloads a repository locally.

**29. What is a remote-tracking branch?** Local reference reflecting last fetched/pushed knowledge of a remote branch.

**30. What is `git stash`?** Temporarily shelves uncommitted changes for later reapplication.

**31. What is a tag?** A named reference to a commit, commonly used for releases.

**32. Annotated vs lightweight tag?** Annotated tags are tag objects with metadata/message; lightweight tags are simple refs.

**33. What does `git diff --staged` show?** Changes staged relative to the current `HEAD`.

**34. What is `git blame`?** Shows the most recent commit associated with each line of a file.

**35. Why is `.gitattributes` useful?** It sets file handling rules such as line endings and merge/diff attributes.

**36. What is an upstream branch?** The tracked branch used for status and default pull/push behavior.

**37. How to change a remote URL?** `git remote set-url origin NEW_URL`.

**38. How to undo staging?** `git restore --staged FILE`.

**39. How to undo a published commit safely?** `git revert HASH` creates a new reversing commit.

**40. Why are LF and CRLF warnings common on Windows?** Git may normalize text line endings between repository and working tree.

### Advanced / DevOps (41–65)

**41. Merge vs rebase?** Merge integrates histories; rebase replays commits with new IDs onto a new base.

**42. Why avoid rebasing shared commits?** It changes commit IDs and disrupts collaborators relying on original history.

**43. Reset vs restore vs revert?** Reset moves refs/index; restore changes file/index content; revert creates an inverse commit.

**44. `reset --soft` vs `--mixed` vs `--hard`?** Soft retains staged changes; mixed unstages but keeps working changes; hard discards local changes to match target.

**45. What is reflog?** Local record of ref/HEAD movements, useful for recovering recently moved commits.

**46. What is detached HEAD?** HEAD points directly at a commit rather than a branch.

**47. What is cherry-pick?** Applying a selected commit's changes onto another branch as a new commit.

**48. What is interactive rebase?** A way to reorder, squash, drop, or edit commits in local history.

**49. What is squash merge?** Integrates the combined feature changes as one new commit on the target branch.

**50. What is `--force-with-lease`?** Conditional force push that checks the remote ref against expected state; safer than unconditional force but still rewrites history.

**51. What is `git bisect`?** Binary search through commits to find the change introducing a regression.

**52. What is `git worktree`?** Additional working directories attached to one repository for parallel branch work.

**53. What is a submodule?** A reference to a specific commit of another repository within a parent repository.

**54. What is sparse checkout?** Checking out only selected repository paths into a working tree.

**55. What are Git objects?** Blob, tree, commit, and annotated tag objects stored by content ID.

**56. What is CI?** Automated integration checks such as linting, testing, and builds after code changes.

**57. What is CD?** Automating delivery or deployment of validated software through defined stages.

**58. What is GitHub Actions?** GitHub's workflow automation platform using event-triggered YAML workflows.

**59. What is a runner?** The machine/environment executing a GitHub Actions job.

**60. Why use protected branches?** To require checks/reviews and reduce accidental or unauthorized changes.

**61. How should GitHub Actions access AWS?** Prefer scoped OIDC federation to an IAM role instead of static AWS keys.

**62. What if AWS credentials were committed?** Revoke/rotate immediately, investigate exposure, and remediate history according to policy.

**63. How do you debug a rejected push?** Check branch/upstream, fetch remote, inspect divergence and repository rules, integrate changes safely.

**64. How do you ensure reproducible releases?** Pin source commit, dependencies, build tooling, and immutable artifact versions/digests.

**65. Explain a Git-based DevOps pipeline.** Developer creates branch → commit/push → PR → CI tests and review → merge → build artifact → approved deployment → monitoring/rollback.

### Scenario practice (66–75)

**66. You accidentally committed the wrong file but did not push.** Inspect `git status` and `git show`; use `git reset --soft HEAD~1` to revise staged content, then recommit, or amend safely.

**67. You pushed a bad commit to main.** Prefer `git revert HASH`, validate, and push the new commit rather than rewriting shared history.

**68. Two engineers changed the same README line.** Resolve merge conflict deliberately, run checks, and commit the resolution.

**69. A teammate pushed while you were editing.** Commit/stash local work, `git fetch`, inspect remote changes, then merge/rebase appropriately.

**70. GitHub says `remote origin already exists`.** Inspect `git remote -v`, update with `git remote set-url` if needed.

**71. A new `.env` was ignored, but an old `.env` remains on GitHub.** Ignore rules do not untrack existing files; stop tracking, rotate exposed secrets, and remediate history.

**72. CI passed locally but failed on GitHub.** Compare OS, tool versions, dependencies, environment variables, permissions, and secrets; inspect workflow logs.

**73. How do you stop a failed release from deploying?** Require successful CI checks, protected environments, approvals, and fail-fast deployment gates.

**74. You need to work on an urgent hotfix while a feature is unfinished.** Commit or stash work, create a hotfix branch (or separate worktree), test, review, merge, then resume.

**75. A commit disappeared after a local reset.** Inspect `git reflog`, create a recovery branch at the prior hash, and verify contents before altering history again.

## 25. Command reference and revision checklist

### Quick cheat sheet

| Task | Command |
|---|---|
| Git version | `git --version` |
| Start repository | `git init -b main` |
| Clone repository | `git clone URL` |
| Show status | `git status -sb` |
| Stage one file | `git add FILE` |
| Stage all | `git add -A` |
| Inspect unstaged diff | `git diff` |
| Inspect staged diff | `git diff --staged` |
| Commit | `git commit -m "message"` |
| View history | `git log --oneline --graph --all` |
| Create branch | `git switch -c feature/name` |
| Change branch | `git switch main` |
| Merge | `git merge feature/name` |
| Download remote updates | `git fetch origin` |
| Update with fast-forward | `git pull --ff-only` |
| Upload commits | `git push` |
| List remotes | `git remote -v` |
| Unstage | `git restore --staged FILE` |
| Discard local tracked-file edits | `git restore FILE` **(destructive)** |
| Revert shared commit | `git revert HASH` |
| Save temporary work | `git stash push -m "WIP"` |
| View stash | `git stash list` |
| Tag release | `git tag -a v1.0.0 -m "Release"` |
| Recover local history | `git reflog` |
| Locate regression | `git bisect` |

### Hands-on completion checklist

- [ ] Explain Git vs GitHub without reading notes.
- [ ] Install Git and configure correct identity.
- [ ] Create, clone, and publish a repository.
- [ ] Explain working directory, index, commits, branches, and remotes.
- [ ] Run `status`, `diff`, `add`, `commit`, `log`, and `push` confidently.
- [ ] Create and merge a feature branch.
- [ ] Resolve a merge conflict in a disposable repository.
- [ ] Explain fetch vs pull vs push.
- [ ] Use `.gitignore` and `.gitattributes` correctly.
- [ ] Undo staging and safely revert a commit.
- [ ] Explain reset dangers and recover via reflog.
- [ ] Open, review, and merge a GitHub PR.
- [ ] Set up GitHub Actions automated tests.
- [ ] Explain safe AWS authentication for CI/CD.
- [ ] Complete mini, intermediate, and advanced projects.
- [ ] Answer 75 interview questions aloud.

## 26. Official documentation

- [Git official documentation](https://git-scm.com/docs)
- [Pro Git book (free)](https://git-scm.com/book/en/v2)
- [GitHub Docs](https://docs.github.com/)
- [GitHub authentication](https://docs.github.com/en/authentication)
- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [GitHub pull requests](https://docs.github.com/en/pull-requests)
- [Git restore](https://git-scm.com/docs/git-restore)
- [Git switch](https://git-scm.com/docs/git-switch)
- [Git reflog](https://git-scm.com/docs/git-reflog)

---

## 🎯 Final learning advice

**First master:** `status → add → commit → push`.  
**Then learn:** `branch → switch → merge → pull request`.  
**Next practice:** `fetch → diff → revert → stash → reflog`.  
**Finally build:** `GitHub Actions → tests → approvals → safe deployment`.

> **Learn → Practice → Make Mistakes Safely → Recover → Document → Automate.**
