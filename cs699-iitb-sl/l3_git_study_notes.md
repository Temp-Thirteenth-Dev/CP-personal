# L3\_Git\_Study\_Notes

## CS 699 — Lecture 3 Study Notes

### Version Control, Git & GitHub

> **Source:** CS 699 – Lec 3, Om Damani, CSE IIT Bombay\
> **Purpose:** Lecture revision + quiz preparation + lab-test/practical preparation

***

## What You Should Be Able to Do

After this lecture, you should be able to:

* Explain what a **Version Control System (VCS)** is and why it is useful.
* Distinguish **Local VCS, Centralised VCS (CVCS), and Distributed VCS (DVCS)**.
* Explain why Git is a **distributed** version control system.
* Create a Git repository and make commits.
* Understand the basic Git flow: **working directory → staging area → repository**.
* Inspect changes using `git status`, `git diff`, and `git diff --staged`.
* Create, switch, compare, merge, and delete branches.
* Explain and resolve a **merge conflict**.
* Understand a **push rejection** and why `git push --force` is dangerous.
* Use `.gitignore`.
* Explain the difference between **Git, GitHub, GitLab, and Sourcetree**.
* Connect a local repository to GitHub and push commits.
* Work collaboratively using branches and Pull Requests.
* Understand the basic Git object model: **blobs, trees, commits**.
* Perform the kinds of Git workflows required by the lecture assignments.

## Version Control

### Definition

A **Version Control System (VCS)** is a tool that records changes made to files over time.

It allows you to:

* Track changes
* Compare versions
* Restore previous versions
* Manage different versions of a project
* Recall a specific version later
* See who changed what
* Undo mistakes

### Why do we need it?

Without version control, collaboration can turn into:

```
report_final
report_final_v2
report_final_NEW
report_final_ACTUAL
report_final_ACTUAL_v3
```

Git provides a systematic history instead.

## Types of Version Control Systems

| Type                       | Main idea                                            | Example        |
| -------------------------- | ---------------------------------------------------- | -------------- |
| **Local VCS**              | History stored in a database on one machine          | RCS            |
| **Centralised VCS (CVCS)** | One server holds the full history                    | CVS, SVN       |
| **Distributed VCS (DVCS)** | Every clone contains the complete repository/history | Git, Mercurial |

### Local VCS

History is stored locally on a single machine.

Example:

* **RCS (1982)** — tracked patch sets per file.

### Centralised VCS

A central server contains the repository history.

Examples:

* CVS (1990)
* SVN / Subversion (2000)

The lecture notes that CVS had no atomic commits, while SVN fixed several CVS flaws.

### Distributed VCS

Every clone is a complete repository containing the history.

Examples:

* Git (2005)
* Mercurial / hg (2005)

{% hint style="info" %}
**Git is DVCS:** a clone is not merely a snapshot; it contains the repository history.
{% endhint %}

## Why Version Control?

### Collaboration without chaos

Multiple people can edit the same project.

Git can merge their work instead of simply overwriting changes.

### Accountability and history

Changes are associated with:

* Person
* Timestamp
* Commit message

The message explains why a change was made.

### Safe experimentation

Branches allow risky work to happen separately.

```
main
 |
 +---- feature branch
          |
          +---- experiment
```

The working `main` branch can remain untouched until the experiment is ready.

### Safety net

Each commit acts as a restore point.

If a change breaks the project, earlier states can be recovered.

## What is Git?

**Git** is a distributed version control system used to track changes in files and manage different versions of a project.

Important facts from the lecture:

* Created by **Linus Torvalds**
* Originally developed in **April 2005**
* Works offline for operations such as:
  * commit
  * branch
  * diff
  * log
* Network is needed for sharing operations such as push/pull.
* Supports branching and merging.
* Multiple developers can work on the same project.
* Git objects are content-addressed by hashes.
* If content changes, its identity/hash changes.

## Why Git?

### Distributed

```bash
git clone <repository>
```

gives you the repository history, not merely the latest snapshot.

Many operations work offline:

```bash
git commit
git branch
git diff
git log
```

{% hint style="info" %}
There is no need for a server for normal local version-control operations. A clone contains the repository inside its `.git` directory.
{% endhint %}

### Branching

Branches are inexpensive and allow independent development.

```
main
 |
 +---- feature/login
 |
 +---- experiment
```

### Content-based identity

Git objects are named using hashes of their contents.

This means:

* Identical content can be stored once.
* Changing content changes its hash.
* Undetected corruption/tampering is detectable.

### Recovery

The lecture highlights:

```bash
git reflog
```

It records previous states that the repository's `HEAD` has been in.

The lecture notes approximately **90 days** as the usual reflog retention period.

## Git's Basic Process Flow

The fundamental mental model is:

```
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Git Repository
```

Think of it as:

```
edit files
   ↓
check changes
   ↓
stage selected changes
   ↓
commit a snapshot
```

### Three important states

#### Working directory

Files you are currently editing.

#### Staging area

Changes selected for the next commit.

#### Repository

Committed history stored by Git.

## Installing and Configuring Git

{% stepper %}
{% step %}
### Install Git

On Linux:

```bash
sudo apt install git
```

Check installation:

```bash
git --version
```
{% endstep %}

{% step %}
### Configure identity

Git attaches a name and email to commits.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Set the default branch name:

```bash
git config --global init.defaultBranch main
```

Check configuration:

```bash
git config --list
```

{% hint style="info" %}
If asked to configure Git, remember:

```
user.name
user.email
init.defaultBranch
```
{% endhint %}
{% endstep %}
{% endstepper %}

## Creating a Local Git Repository

{% stepper %}
{% step %}
### Create and enter a project folder

```bash
mkdir my-project
cd my-project
```
{% endstep %}

{% step %}
### Initialize Git

```bash
git init
```

Check repository state:

```bash
git status
```
{% endstep %}

{% step %}
### Create files

```bash
echo "# My Projects readme file" > README.md
echo "print('hello')" > app.py
```

Check again:

```bash
git status
```

At this stage, the newly created files are **untracked**.
{% endstep %}
{% endstepper %}

## Stage and Commit

### Stage

```bash
git add README.md app.py
```

### Commit

```bash
git commit -m "Initial commit: add README and app"
```

### View history

```bash
git log --oneline
```

The basic sequence is:

```bash
git status
git add <files>
git commit -m "message"
git log --oneline
```

## Modifying an Existing File

Suppose:

```bash
echo "print('hello again')" >> app.py
```

Now inspect the modification:

```bash
git diff
```

The lecture uses:

```
+ added
- removed
```

Then:

```bash
git add app.py
git commit -m "Update greeting in app.py"
```

View compact history:

```bash
git log --oneline --graph
```

## `git status` — Extremely Important

Use:

```bash
git status
```

to understand the current state of your repository.

It can show things such as:

* Current branch
* Untracked files
* Modified files
* Staged changes
* Changes ready for commit

{% hint style="info" %}
When confused, run:

```bash
git status
```

before doing anything else.
{% endhint %}

## `git diff` vs `git diff --staged`

{% tabs %}
{% tab title="Changes not yet staged" %}
```bash
git diff
```

Shows changes in the working directory that have **not** been staged.
{% endtab %}

{% tab title="Changes already staged" %}
```bash
git diff --staged
```

Shows what is currently prepared for the next commit.
{% endtab %}
{% endtabs %}

```
working tree changes → git diff

staged changes       → git diff --staged
```

## `.gitignore`

A `.gitignore` file tells Git about files/directories that should be ignored.

Lecture example:

```bash
mkdir "node_modules"
printf "node_modules" > .gitignore
```

Then:

```bash
git add .gitignore
git commit -m "Add gitignore"
```

Some generated/dependency files should not be included in version control.

## Branches

A branch allows development to happen separately from another line of development.

| Task                              | Command                       |
| --------------------------------- | ----------------------------- |
| Create a branch                   | `git branch <branch-name>`    |
| Create and switch to a new branch | `git switch -c <branch-name>` |
| Switch branches                   | `git switch <branch-name>`    |
| Switch to main                    | `git switch main`             |
| List branches                     | `git branch`                  |
| Show current branch               | `git branch --show-current`   |
| List local and remote branches    | `git branch -a`               |

Example:

```bash
git switch -c feature/new699branch
```

## Branch Workflow

```bash
git switch -c feature/new699branch
```

Make a change:

```bash
echo "print('add file to the new branch')" >> app.py
```

Commit:

```bash
git commit -am "Add branch-specific line"
```

Then switch back:

```bash
git switch main
```

The branch-specific change does not automatically become part of `main`.

## Merging Branches

Suppose you are on `main`:

```bash
git switch main
```

Merge another branch:

```bash
git merge <branch-name>
```

Example:

```bash
git merge feature/new699branch
```

After merging, the changes from that branch are incorporated into `main`.

### Delete a local branch

After merging:

```bash
git branch -d <branch-name>
```

### Inspect the history

```bash
git log --oneline --graph --all
```

## Merge Conflicts

A merge conflict can occur when:

* Two branches modify the **same part of the same file**
* Git cannot determine automatically which version should remain

Git can often merge:

* Different files
* Different parts of the same file

A conflict usually occurs when both branches change overlapping content.

### What Git does

Git:

1. Pauses the merge.
2. Marks the file as conflicted.
3. Places both versions into the file using conflict markers.

You must manually decide the final contents.

## Resolving a Merge Conflict

```
Branch A change
       \
        +---- conflict ----> manually choose final code
       /
Branch B change
```

Git cannot decide the desired final content for you.

{% stepper %}
{% step %}
### Inspect the conflicted file

Find the conflicted file and inspect the changes.
{% endstep %}

{% step %}
### Decide the final code

Choose what the final contents should be.
{% endstep %}

{% step %}
### Remove or modify conflict markers

Edit the file, remove or modify the markers, and save the corrected file.
{% endstep %}

{% step %}
### Stage and commit the resolution

Stage the resolved file and commit the resolution.
{% endstep %}

{% step %}
### Continue the workflow

Continue working with the resolved version.
{% endstep %}
{% endstepper %}

The lecture also demonstrates resolving conflicts through GitHub.

## Push Rejection

A different problem is a **push rejection**.

```
Remote repository moved ahead
          ↓
Your local repository is behind
          ↓
Your push could overwrite remote commits
          ↓
Git rejects the push
```

The lecture's example:

```
! [rejected] main -> main (fetch first)
```

### Why does it happen?

Someone pushed to `main` after your last pull.

Your local history does not contain that new remote update.

{% hint style="warning" %}
**Do not fix a normal push rejection with `git push --force`.**

The lecture specifically warns that this can delete teammates' work.
{% endhint %}

## Git Cheat Sheet

| Task                         | Command                           |
| ---------------------------- | --------------------------------- |
| Check Git version            | `git --version`                   |
| Show config                  | `git config --list`               |
| Create repository            | `git init`                        |
| Check status                 | `git status`                      |
| Stage file                   | `git add <file>`                  |
| Stage everything             | `git add .`                       |
| Commit                       | `git commit -m "message"`         |
| View history                 | `git log`                         |
| Compact history              | `git log --oneline`               |
| History graph                | `git log --graph`                 |
| Full graph                   | `git log --oneline --graph --all` |
| View unstaged changes        | `git diff`                        |
| View staged changes          | `git diff --staged`               |
| Show commit                  | `git show <commit>`               |
| List branches                | `git branch`                      |
| Current branch               | `git branch --show-current`       |
| All branches                 | `git branch -a`                   |
| Create branch                | `git branch <name>`               |
| Create + switch              | `git switch -c <name>`            |
| Switch branch                | `git switch <name>`               |
| Merge                        | `git merge <name>`                |
| Delete local branch          | `git branch -d <name>`            |
| Fetch remote changes         | `git fetch`                       |
| Download + integrate         | `git pull`                        |
| Upload commits               | `git push`                        |
| Temporarily save changes     | `git stash`                       |
| Restore stash                | `git stash pop`                   |
| Restore/discard file changes | `git restore <file>`              |
| Unstage file                 | `git restore --staged <file>`     |
| Reverse an earlier commit    | `git revert <commit>`             |
| Move HEAD/branch             | `git reset <commit>`              |
| View previous HEAD states    | `git reflog`                      |
| Show line authorship         | `git blame <filename>`            |
| Create tag                   | `git tag <tag-name>`              |
| Add remote                   | `git remote add origin <URL>`     |
| Compare branches             | `git diff main <branch-name>`     |

## `revert` vs `reset`

{% tabs %}
{% tab title="git revert" %}
```bash
git revert <commit>
```

Creates a **new commit** that reverses an earlier commit.

```
A → B → C → reverse-C
```

The old history remains.
{% endtab %}

{% tab title="git reset" %}
```bash
git reset <commit>
```

Moves the branch/HEAD to another commit.

```
A → B → C
    ↑
   reset
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**revert** creates a new reversing commit; **reset** moves HEAD/branch.
{% endhint %}

## `git stash`

Temporarily save uncommitted changes:

```bash
git stash
```

Restore them:

```bash
git stash pop
```

Useful when you have unfinished work but need to temporarily change branches or work on something else.

## Git Object Model

Git internally stores objects.

### Blob

A **blob** stores file contents.

{% hint style="info" %}
A blob does not store the filename/path.
{% endhint %}

### Tree

A **tree** represents a directory listing.

It maps:

```
name → blob
```

and can also point to other trees.

### Commit

A commit contains information including:

* One tree
* Parent
* Author
* Message

```
Commit
 ├── tree
 ├── parent
 ├── author
 └── message
```

### Hashing

Git objects are named by hashes of their contents.

This helps explain:

* Content identity
* Deduplication
* Integrity
* Why changing old history changes subsequent hashes

## Git vs GitHub vs GitLab vs Sourcetree

{% tabs %}
{% tab title="Git" %}
Git is the actual version-control software.

It runs on your machine and performs:

* commits
* branches
* merges
* history management

Git can work without an internet connection.
{% endtab %}

{% tab title="GitHub" %}
GitHub is a hosting/collaboration platform.

It provides things such as:

* Cloud-hosted repositories
* Pull Requests
* Code review
* Issues
* Discussions
* GitHub Actions
* Permissions/branch rules
{% endtab %}

{% tab title="GitLab" %}
GitLab is a competing platform.

The lecture highlights:

* Self-hosting
* CI/CD
* Container registry
* Security scanning
* Deployment
* Full lifecycle DevOps platform
{% endtab %}

{% tab title="Sourcetree" %}
Sourcetree is a GUI client.

It provides a visual interface to Git.

For example:

```
git log --graph
```

can be viewed as a visual commit tree.

{% hint style="info" %}
Sourcetree is **not** a version-control system. It is a front-end that runs Git commands.
{% endhint %}
{% endtab %}
{% endtabs %}

## Why GitHub When We Have Git?

| Git                            | GitHub                   |
| ------------------------------ | ------------------------ |
| Lives on your machine          | Hosts a copy online      |
| Local version control          | Online collaboration     |
| No built-in Pull Requests      | Pull Requests            |
| No built-in issue tracking     | Issues/project features  |
| No built-in permissions system | Permissions/branch rules |
| Local commits                  | Shared remote repository |
| No GitHub Actions              | GitHub Actions           |

```
Git
 ↓
version control

GitHub
 ↓
hosting + collaboration
```

## Starting a GitHub Project

{% stepper %}
{% step %}
### Create a GitHub account

1. Go to GitHub.
2. Sign up.
3. Choose username.
4. Verify email.
5. Optionally enable 2FA.
{% endstep %}

{% step %}
### Configure local Git

```bash
git --version

git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main

git config --list
```
{% endstep %}
{% endstepper %}

## Authentication

{% tabs %}
{% tab title="Option A — GitHub CLI" %}
```bash
gh auth login
```

Follow the prompts and authenticate through the browser.
{% endtab %}

{% tab title="Option B — SSH key" %}
Generate:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

Display public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Add the public key through GitHub:

```
Settings
→ SSH and GPG keys
→ New SSH key
```

Test:

```bash
ssh -T git@github.com
```

The lecture expects a successful authentication message.
{% endtab %}
{% endtabs %}

## Creating a GitHub Repository

{% stepper %}
{% step %}
### Create a repository on GitHub

1. Create a new repository.
2. Choose repository name.
3. Choose visibility:
   * Public
   * Private
4. Optionally add README.
5. Optionally add `.gitignore`.
{% endstep %}

{% step %}
### Clone the repository

```bash
git clone git@github.com:yourname/my-project.git
cd my-project
```

Check remotes:

```bash
git remote -v
```
{% endstep %}
{% endstepper %}

## Adding a Remote

A remote connects the local repository with a remote repository.

```bash
git remote add origin <URL>
```

Example:

```bash
git remote add origin https://github.com/username/project.git
```

Check:

```bash
git remote -v
```

```
origin = conventional name for the remote repository
```

## Pushing to GitHub

After creating or modifying files:

```bash
git status
git add .
git commit -m "added index"
```

Then push:

```bash
git push -u origin main
```

The `-u` sets the upstream relationship for the local branch.

After that, later pushes can generally use:

```bash
git push
```

## Pulling from GitHub

To get and integrate remote changes:

```bash
git pull
```

or explicitly:

```bash
git pull origin main
```

```
Remote repository
       ↓
     pull
       ↓
Local repository
```

The lecture uses pulling when collaborators have made newer changes.

## Collaborative Feature-Branch Workflow

```bash
git switch -c feature/branchname
```

Make changes.

```bash
git add .
git commit -m "Add login validation"
```

Push the branch:

```bash
git push -u origin feature/login
```

Then create a Pull Request on GitHub.

## Pull Requests

A Pull Request allows a proposed change to be reviewed before merging.

{% stepper %}
{% step %}
### Open the Pull Request

Open the repository on GitHub, click **Pull requests**, and open the teammate's Pull Request.
{% endstep %}

{% step %}
### Review changes

Select **Files changed**, inspect the changes, and add comments if needed.
{% endstep %}

{% step %}
### Approve the Pull Request

Select **Review changes → Approve**.
{% endstep %}

{% step %}
### Merge the Pull Request

Click **Merge pull request**, then confirm merge.
{% endstep %}

{% step %}
### Update local repository

Optionally delete the feature branch, then update the local repository.
{% endstep %}
{% endstepper %}

```
feature branch
      ↓
    push
      ↓
Pull Request
      ↓
review
      ↓
approve
      ↓
merge
      ↓
main
```

## Two-Person Collaborative Workflow

### Student 1

* Create GitHub repository.
* Add Student 2 as collaborator.

### Both students

{% stepper %}
{% step %}
### Clone repository

Clone the repository.
{% endstep %}

{% step %}
### Work on different files

Modify or create different files.
{% endstep %}

{% step %}
### Stage and commit

Stage changes and commit.
{% endstep %}

{% step %}
### Push and pull

Push changes and pull the latest changes.
{% endstep %}
{% endstepper %}

### Feature branch

{% columns %}
{% column %}
#### One teammate

1. Creates feature branch.
2. Makes changes.
3. Pushes branch.
4. Creates Pull Request.
{% endcolumn %}

{% column %}
#### Repository owner

1. Reviews changes.
2. Accepts/merges Pull Request.
{% endcolumn %}
{% endcolumns %}

After merge, both students update their local `main` branch.

## Creating a Merge Conflict for Practice

Both students should start from the latest version.

```
Student A                 Student B
    |                         |
modify same line         modify same line
    |                         |
   push                      push
    |                         |
    +------ conflict --------+
```

{% stepper %}
{% step %}
### Pull the latest changes

```bash
git pull origin main
```
{% endstep %}

{% step %}
### Identify the conflict

Identify the conflict in the file.
{% endstep %}

{% step %}
### Resolve manually

Manually resolve the conflict and ensure both required changes are present.
{% endstep %}

{% step %}
### Stage and commit

Stage the resolved file and commit the resolution.
{% endstep %}

{% step %}
### Push and verify

Push the resolved version and verify that both required changes are present.
{% endstep %}
{% endstepper %}

## Git Bundle — Backup Without a Server

Git can bundle an entire repository into one file.

```bash
git bundle create ../my-project.bundle --all
```

Restore options from the lecture include:

```bash
git bundle unbundle project.bundle
```

or:

```bash
git clone project.bundle my-project
```

This provides a way to move or back up the repository without a Git server.

## High-Value Command Flows

{% tabs %}
{% tab title="New local repository" %}
```bash
mkdir my-project
cd my-project
git init

# create files

git status
git add .
git commit -m "Initial commit"

git log --oneline
```
{% endtab %}

{% tab title="Modify and commit" %}
```bash
# modify file

git status
git diff

git add <file>
git diff --staged

git commit -m "Describe change"
```
{% endtab %}

{% tab title="Branch and merge" %}
```bash
git switch -c feature/test

# modify files

git add .
git commit -m "Add feature"

git switch main
git merge feature/test

git log --oneline --graph --all
```
{% endtab %}

{% tab title="GitHub project" %}
```bash
git clone <URL>
cd <project>

git status
git add .
git commit -m "Update project"

git push -u origin main
```
{% endtab %}

{% tab title="Feature branch + Pull Request" %}
```bash
git pull origin main

git switch -c feature/login

# modify files

git add .
git commit -m "Add login validation"

git push -u origin feature/login
```

```
GitHub → Pull Request → Review → Approve → Merge
```

Finally:

```bash
git switch main
git pull origin main
```
{% endtab %}
{% endtabs %}

## Troubleshooting Patterns

<details>

<summary>Why isn't my file in the commit?</summary>

Check:

```bash
git status
```

Then stage:

```bash
git add <file>
```

Then commit:

```bash
git commit -m "message"
```

</details>

<details>

<summary>What exactly did I change?</summary>

Use:

```bash
git diff
```

If already staged:

```bash
git diff --staged
```

</details>

<details>

<summary>Which branch am I on?</summary>

```bash
git branch --show-current
```

</details>

<details>

<summary>What branches exist?</summary>

```bash
git branch
```

For local and remote branches:

```bash
git branch -a
```

</details>

<details>

<summary>Why did my merge stop?</summary>

Likely a merge conflict.

Inspect the conflicted file, resolve it manually, stage it, and commit the resolution.

</details>

<details>

<summary>Why was my push rejected?</summary>

The remote repository may contain commits that your local repository does not have.

Do **not** blindly force-push.

The lecture's recommended learning direction is to understand the push-rejection resolution workflow and update your local history.

</details>

## Quiz Preparation — Definitions

<details>

<summary>Q1. What is a VCS?</summary>

A system that records file changes over time so versions can be tracked, compared, restored, and managed.

</details>

<details>

<summary>Q2. What type of VCS is Git?</summary>

Distributed Version Control System (DVCS).

</details>

<details>

<summary>Q3. What does <code>git init</code> do?</summary>

Initializes a Git repository in a project directory.

</details>

<details>

<summary>Q4. What does <code>git add</code> do?</summary>

Stages changes for the next commit.

</details>

<details>

<summary>Q5. What does <code>git commit</code> do?</summary>

Records staged changes as a commit in the repository history.

</details>

<details>

<summary>Q6. What does <code>git status</code> do?</summary>

Shows the current state of the working tree/staging area and repository information.

</details>

<details>

<summary>Q7. What does <code>git diff</code> show?</summary>

Unstaged changes.

</details>

<details>

<summary>Q8. What does <code>git diff --staged</code> show?</summary>

Changes that have been staged.

</details>

<details>

<summary>Q9. Why use branches?</summary>

To develop or experiment separately without directly affecting another branch.

</details>

<details>

<summary>Q10. What causes a merge conflict?</summary>

Git cannot automatically reconcile overlapping changes, commonly when branches modify the same part of a file.

</details>

<details>

<summary>Q11. What is GitHub?</summary>

A hosting/collaboration platform built around Git repositories.

</details>

<details>

<summary>Q12. Is Sourcetree a VCS?</summary>

No. It is a GUI client/front-end for Git.

</details>

<details>

<summary>Q13. What does <code>git pull</code> do?</summary>

Downloads remote changes and integrates them into the local repository.

</details>

<details>

<summary>Q14. What does <code>git push</code> do?</summary>

Uploads local commits to a remote repository.

</details>

<details>

<summary>Q15. What is <code>origin</code>?</summary>

The conventional name used for a remote repository.

</details>

## Quiz Preparation — Differentiate

{% tabs %}
{% tab title="Git vs GitHub" %}
```
Git
= version-control software

GitHub
= online hosting + collaboration platform
```
{% endtab %}

{% tab title="Git vs Sourcetree" %}
```
Git
= version-control system

Sourcetree
= GUI client that runs Git
```
{% endtab %}

{% tab title="GitHub vs GitLab" %}
Both provide repository hosting/collaboration features.

```
GitHub → Pull Requests, Actions, collaboration

GitLab → self-hosting + broader DevOps lifecycle features
```
{% endtab %}

{% tab title="git revert vs git reset" %}
```
revert → creates a new commit reversing an earlier commit

reset  → moves HEAD/branch to another commit
```
{% endtab %}

{% tab title="git diff vs git diff --staged" %}
```
diff          → unstaged changes

diff --staged → staged changes
```
{% endtab %}

{% tab title="git fetch vs git pull" %}
```
fetch → download remote changes

pull  → download + integrate
```
{% endtab %}
{% endtabs %}

## Lab-Test Must-Know Commands

```bash
git init
git status

git add .
git commit -m "message"

git log --oneline
git log --oneline --graph --all

git diff
git diff --staged

git branch
git branch --show-current
git branch -a

git switch -c feature/name
git switch main
git merge feature/name

git remote -v
git remote add origin <URL>

git push -u origin main
git pull origin main

git push -u origin feature/name

git stash
git stash pop

git restore <file>
git restore --staged <file>

git revert <commit>
git reset <commit>

git reflog
```

## Lab-Test Practice Task 1 — Basic Git

Create a project:

```bash
mkdir git-practice
cd git-practice
git init
```

Create:

```
README.md
app.py
```

{% stepper %}
{% step %}
### Check status

Check the repository status.
{% endstep %}

{% step %}
### Stage and commit both files

Stage both files and commit them.
{% endstep %}

{% step %}
### Display commit history

Display commit history.
{% endstep %}

{% step %}
### Modify `app.py`

Modify `app.py` and display the difference.
{% endstep %}

{% step %}
### Stage the modification

Stage the modification and display the staged difference.
{% endstep %}

{% step %}
### Commit and display compact history

Commit again and display compact history.
{% endstep %}
{% endstepper %}

Commands you should be able to produce:

```bash
git status
git add .
git commit -m "Initial commit"
git log --oneline

git diff
git add app.py
git diff --staged
git commit -m "Update app"
git log --oneline
```

## Lab-Test Practice Task 2 — Branching

Starting from a repository:

{% stepper %}
{% step %}
### Create and switch to a feature branch

Create a feature branch and switch to it.
{% endstep %}

{% step %}
### Modify and commit

Modify a file and commit the change.
{% endstep %}

{% step %}
### Switch back to `main`

Switch back to `main` and observe the difference.
{% endstep %}

{% step %}
### Merge the feature branch

Merge the feature branch into `main`.
{% endstep %}

{% step %}
### Display graph and delete branch

Display the graph and delete the feature branch.
{% endstep %}
{% endstepper %}

Expected workflow:

```bash
git switch -c feature/test

# edit

git add .
git commit -m "Feature change"

git switch main
git merge feature/test

git log --oneline --graph --all

git branch -d feature/test
```

## Lab-Test Practice Task 3 — GitHub

Starting from a GitHub repository:

{% stepper %}
{% step %}
### Clone the repository

```bash
git clone <URL>
cd <project>
```
{% endstep %}

{% step %}
### Check the remote

```bash
git remote -v
```
{% endstep %}

{% step %}
### Modify or create a file

Modify or create a file.
{% endstep %}

{% step %}
### Commit and push to `main`

```bash
git add .
git commit -m "Update project"
git push -u origin main
```
{% endstep %}
{% endstepper %}

## Lab-Test Practice Task 4 — Feature Branch + PR

```bash
git pull origin main
git switch -c feature/test

# modify files

git add .
git commit -m "Add feature"
git push -u origin feature/test
```

```
feature/test
     ↓
Pull Request
     ↓
Review
     ↓
Approve
     ↓
Merge
```

Then locally:

```bash
git switch main
git pull origin main
```

## Lab-Test Practice Task 5 — Merge Conflict

Two people modify the same line.

After one person pushes, the other should:

```bash
git pull origin main
```

{% stepper %}
{% step %}
### Open the conflicted file

Open the conflicted file.
{% endstep %}

{% step %}
### Identify conflict markers

Identify Git's conflict markers.
{% endstep %}

{% step %}
### Decide final contents

Decide the final desired contents.
{% endstep %}

{% step %}
### Remove markers and save

Remove the conflict markers and save the file.
{% endstep %}

{% step %}
### Stage, commit, and push

```bash
git add <resolved-file>
git commit -m "Resolve merge conflict"
git push
```
{% endstep %}
{% endstepper %}

## Assignment 1 — Website Version Control

{% stepper %}
{% step %}
### Create project repository

Create a Git repository for your HTML website from Assignment 1.
{% endstep %}

{% step %}
### Create project folder and initialize Git

Create the project folder using the shell and initialize it as a Git repository.
{% endstep %}

{% step %}
### Add HTML files and make an initial commit

Add HTML files and create an initial commit.
{% endstep %}

{% step %}
### Modify the HTML page

Modify the HTML page and create another commit.
{% endstep %}

{% step %}
### Create an alternative branch

Create a new branch for an alternative version, make different changes, and commit the branch changes.
{% endstep %}

{% step %}
### Switch branches and inspect history

Switch between branches, observe different webpage versions, and display Git commit history.
{% endstep %}
{% endstepper %}

Minimum skills being tested:

```
init
add
commit
branch
switch
log
```

## Assignment 2 — Version Control of a Project

Create a new project and add the CSV/text files from Lecture 1.

You should be able to:

1. Initialize the repository.
2. Add files.
3. Make an initial commit.
4. Make further changes and commits.
5. Create a new branch.
6. Make different changes on both branches.
7. Add files/folders on the new branch.
8. Switch between branches.
9. Compare files/folders.
10. Compare commits/changes.
11. Merge the branches.
12. Observe whether Git merges automatically or produces a conflict.
13. Display the final history.

## Rapid Revision — One Page

### Core idea

```
VCS
 ↓
tracks versions/history

Git
 ↓
distributed VCS

GitHub
 ↓
hosting + collaboration
```

### Core workflow

```
EDIT
 ↓
git status
 ↓
git diff
 ↓
git add
 ↓
git diff --staged
 ↓
git commit
 ↓
git log
```

### Branch workflow

```
git switch -c feature/x
 ↓
edit
 ↓
git add .
 ↓
git commit
 ↓
git switch main
 ↓
git merge feature/x
```

### Remote workflow

```
clone
 ↓
edit
 ↓
add
 ↓
commit
 ↓
push
```

### Collaboration

```
main
 ↓
feature branch
 ↓
push
 ↓
Pull Request
 ↓
review
 ↓
merge
 ↓
pull updated main
```

### Conflict

```
same part changed
       ↓
merge/pull
       ↓
conflict
       ↓
manual resolution
       ↓
git add
       ↓
commit
       ↓
push
```

## Final Self-Check

Before the quiz/lab test, make sure you can explain each of these **without notes**:

* [ ] What is version control?
* [ ] Local vs centralised vs distributed VCS
* [ ] Why Git is distributed
* [ ] Working directory vs staging area vs repository
* [ ] `git init`
* [ ] `git status`
* [ ] `git add`
* [ ] `git commit`
* [ ] `git diff`
* [ ] `git diff --staged`
* [ ] `.gitignore`
* [ ] Creating/switching branches
* [ ] Merging
* [ ] Merge conflicts
* [ ] Push rejection
* [ ] Why not blindly use `git push --force`
* [ ] `git fetch` vs `git pull`
* [ ] `git push`
* [ ] `git stash`
* [ ] `git restore`
* [ ] `git revert` vs `git reset`
* [ ] `git reflog`
* [ ] Git object model: blob/tree/commit
* [ ] Git vs GitHub
* [ ] GitHub vs GitLab
* [ ] Sourcetree
* [ ] Remote `origin`
* [ ] GitHub authentication
* [ ] `git clone`
* [ ] `git remote add origin`
* [ ] `git push -u origin main`
* [ ] Feature branches
* [ ] Pull Requests
* [ ] Collaborative workflow
* [ ] Manual merge-conflict resolution

## Ultra-Short Memory Map

```
                    VERSION CONTROL
                          |
                         Git
                          |
          +---------------+---------------+
          |               |               |
       History         Branches        Collaboration
          |               |               |
       commit           switch          GitHub
       log              merge              |
       diff             conflict            |
                                      Pull Request
                                      review/merge


Git local workflow:
working directory
       ↓ add
staging area
       ↓ commit
repository


Remote workflow:
local repository
       ↓ push
GitHub
       ↓ pull
local repository
```

### Source / Further Reading

The lecture references:

* **Pro Git** — Scott Chacon and Ben Straub
* **Version Control with Git** — Prem Kumar Ponuthurai and Jon Loeliger
* Git book: `https://git-scm.com/book/en/v2`
* GitHub documentation: `https://docs.github.com/`

### Note on Scope

These notes intentionally follow the **L3 lecture material** and its terminology. Commands and explanations are organized to emphasize what is likely to matter for **quiz recall and hands-on lab execution**, rather than adding unrelated Git features.
