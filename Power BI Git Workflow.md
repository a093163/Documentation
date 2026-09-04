# Power BI Git Workflow

## Prerequisites

### Setting up your Pygressive workspace

- https://dss/docs/pygressive/getting_started/

### Getting your cloud password / token

1. Go to https://dev.azure.com/progcloud
2. Select the person/gear icon next to your photo and select **Personal Access Tokens**
3. Select **Pygressive GIT Cloud Tokens**
4. Select **Regenerate**
5. Copy and save the token somewhere secure, or you will need to regenerate a new one later

---

## 1. Go to your repo using Pygressive

- Setting up Pygressive
- Going to Pygressive

---

## 2. Figure out where you are / were

```bash
git status
```

See glossary: [git status](#git-status)

- Shows your current branch
- Shows whether you have staged, unstaged, or untracked changes

Related help:
- How to interpret git status output

---

## 3. Pull latest version from `main`

```bash
git checkout main
git pull origin main
```

See glossary: [git checkout](#git-checkout), [git pull](#git-pull)

- `git checkout main` switches you to local `main`
- `git pull origin main` updates local `main` from remote `main`

> Note: you may have to enter your Azure DevOps credentials.

Related help:
- Where do I find this password

---

## 4. Create a branch for your next task

```bash
git switch -c feature/WorkItemId-ReportName-Action
```

See glossary: [git switch -c](#git-switch-c)

- Creates a new branch from your current branch
- Immediately switches you to it

### If the branch already exists

```bash
git checkout feature/WorkItemId-ReportName-Action
```

See glossary: [git checkout](#git-checkout)

- Switches to an existing local branch

### If you mistyped the branch name, rename it

```bash
git branch -m new-branch-name
```

See glossary: [git branch -m](#git-branch-m)

- Renames your current local branch

---

## 5. Open the Power BI report

- Make sure you are working in the correct repo branch
- Make necessary changes / updates / enhancements

---

## 6. In Pygressive, check what changed

```bash
git status
```

See glossary: [git status](#git-status)

- Confirms which files changed
- Helps you spot tracked vs untracked files

---

## 7. Stage changes for the report

```bash
git add "_solution_.Report" "_solution_.SemanticModel"
```

See glossary: [git add](#git-add)

- Stages only the report/model folders you name

> Note: use your exact folder names.

### If you are doing a bulk update and want all unstaged changes

```bash
git add .
```

See glossary: [git add](#git-add)

- Stages everything changed under the current folder

---

## 8. Commit changes

```bash
git commit -m "describe changes"
```

See glossary: [git commit -m](#git-commit-m)

- Creates a commit with a description of your changes

---

## 9. Push your branch to the remote repo

```bash
git branch
git push -u origin _branchname_
```

See glossary: [git branch](#git-branch), [git push -u](#git-push-u)

- `git branch` shows your local branches
- `git push -u origin _branchname_` pushes the branch and links it to remote

### If you already pushed the branch before

```bash
git push
```

See glossary: [git push](#git-push)

- Pushes new local commits to the already-linked remote branch

> Note: you may have to enter your Azure DevOps credentials.

Related help:
- Where do I find this password

---

## 10. Go to the remote repo

- Create pull request
- Complete pull request
- Review elevated changes

This PR goes from your feature branch into `dev`.

---

## 11. Clean up local feature branch

```bash
git fetch --prune origin
git branch
git branch -d _branchname_
```

See glossary: [git fetch --prune](#git-fetch-prune), [git branch](#git-branch), [git branch -d](#git-branch-d)

- Refreshes remote branch info
- Removes deleted remote references
- Deletes your local branch after the PR is complete

---

## 12. Get latest from remote

```bash
git fetch origin
```

See glossary: [git fetch](#git-fetch)

- Downloads latest remote branch info without changing your files

---

## 13. Switch to `main` (prod)

```bash
git switch main
```

See glossary: [git switch](#git-switch)

- Switches to your local `main` branch

---

## 14. Update local `main` (prod)

```bash
git pull origin main
```

See glossary: [git pull](#git-pull)

- Brings local `main` up to date with remote `main`

---

## 15. Create a release branch from `main` (prod)

```bash
git switch -c release/WorkItemId-ReportName-Action
```

See glossary: [git switch -c](#git-switch-c)

- Creates a new release branch from `main`
- Switches you to it

---

## 16. Bring over the specific report from `dev`

```bash
git restore --source origin/dev "powerbi/solutions/IntelliRouter Users Dashboard.Report"
git restore --source origin/dev "powerbi/solutions/IntelliRouter Users Dashboard.SemanticModel"
```

See glossary: [git restore --source](#git-restore-source)

- Copies the report and semantic model from `origin/dev` into your current release branch
- Lets you elevate one report without merging all of `dev`

---

## 17. Review what changed

```bash
git status
```

See glossary: [git status](#git-status)

- Confirms only the intended changes are present

---

## 18. Remove any local Power BI junk if it appeared

```bash
git rm --cached -r "powerbi/solutions/IntelliRouter Users Dashboard.SemanticModel/.pbi"
```

See glossary: [git rm --cached -r](#git-rm-cached-r)

- Stops tracking the local `.pbi` folder
- Keeps the folder on your machine
- Useful for things like `.pbi/cache.abf`

---

## 19. Commit the release branch

```bash
git add .
git commit -m "Release _reportname_ from dev to prod"
```

See glossary: [git add](#git-add), [git commit -m](#git-commit-m)

- Stages the release changes
- Creates the release commit

---

## 20. Push the release branch

```bash
git push origin -u release/WorkItemId-ReportName-Action
```

See glossary: [git push -u](#git-push-u)

- Pushes the release branch to remote
- Links it for future pushes

---

## 21. Go to the remote repo

- Create pull request
- Important: the repo may default the PR target to `dev`; change it to `main`
- Complete pull request
- Go to the `Claims-BI-Prod` workspace
- Open source control
- In Updates, confirm your report is there and select **Update all**
- Review elevated changes

This PR goes from your release branch into `main`.

---

## 22. Refresh your local repo and remove deleted remote branches

```bash
git fetch --prune origin
```

See glossary: [git fetch --prune](#git-fetch-prune)

- Updates remote branch info
- Removes deleted remote branch references

---

## 23. Switch to `main` and update it

```bash
git switch main
git pull origin main
```

See glossary: [git switch](#git-switch), [git pull](#git-pull)

- Switches back to local `main`
- Pulls the completed prod PR changes down locally

---

## 24. Delete your local release branch

```bash
git branch
git branch -d release/WorkItemId-ReportName-Action
```

See glossary: [git branch](#git-branch), [git branch -d](#git-branch-d)

- Shows your branches
- Deletes the local release branch after the PR is complete

> Note: if Azure DevOps used a squash merge, Git may warn that the branch is not merged to `HEAD` even though the PR completed. After confirming the PR is merged and `main` has the changes, you can use:

```bash
git branch -D release/WorkItemId-ReportName-Action
```

See glossary: [git branch -D](#git-branch-capital-d)

---

## You are ready for your next project

---

# Git Command Glossary

Common options below are not every possible Git option. They are the ones most likely to be useful in this workflow.

<a id="git-status"></a>
## git status

```bash
git status
```

### What it does
Shows:
- your current branch
- staged changes
- unstaged changes
- untracked files

### Syntax
```bash
git status
```

### Common options
- `-s` or `--short` = compact output
- `-b` or `--branch` = include branch information
- `--ignored` = show ignored files
- `-u` = control how untracked files are shown

### Examples
```bash
git status
git status --short
git status --ignored
```

---

<a id="git-checkout"></a>
## git checkout

```bash
git checkout <branch>
```

### What it does
Switches to an existing branch.

### Syntax
```bash
git checkout <branch>
```

### Common options
- `-b <branch>` = create a new branch and switch to it
- `-- <path>` = restore a file from another branch or commit
- `-f` = force checkout, discarding local changes
- `--track` = create a branch that tracks a remote branch

### Examples
```bash
git checkout main
git checkout feature/12345-report-update
git checkout -b feature/12345-report-update
git checkout origin/dev -- "powerbi/solutions/My Report.Report"
```

---

<a id="git-pull"></a>
## git pull

```bash
git pull origin <branch>
```

### What it does
Gets the latest changes from a remote branch and applies them to your local branch.

### Syntax
```bash
git pull origin <branch>
```

### Common options
- `--rebase` = reapply your local commits on top of pulled changes
- `--ff-only` = only pull if Git can fast-forward
- `--no-rebase` = force merge behavior instead of rebase
- `origin <branch>` = specify the remote and branch

### Examples
```bash
git pull origin main
git pull --rebase origin dev
git pull --ff-only origin main
```

---

<a id="git-switch"></a>
## git switch

```bash
git switch <branch>
```

### What it does
Switches to an existing local branch.

### Syntax
```bash
git switch <branch>
```

### Common options
- `-` = switch back to the previous branch
- `--detach` = switch to a commit without being on a branch
- `-c <branch>` = create a new branch and switch to it
- `-C <branch>` = create or reset a branch and switch to it

### Examples
```bash
git switch main
git switch dev
git switch -
```

---

<a id="git-switch-c"></a>
## git switch -c

```bash
git switch -c <new-branch>
```

### What it does
Creates a new branch from your current branch and switches you to it.

### Syntax
```bash
git switch -c <new-branch>
git switch -c <new-branch> <start-point>
```

### Common options
- `-c` = create a new branch, then switch to it
- `<start-point>` = branch or commit to create it from
- `--track` = set upstream tracking when creating from a remote branch

### Examples
```bash
git switch -c feature/12345-Users-Dashboard-Update
git switch -c release/12345-Users-Dashboard-Update origin/main
```

---

<a id="git-branch"></a>
## git branch

```bash
git branch
```

### What it does
Shows local branches.

### Syntax
```bash
git branch
```

### Common options
- `-a` = show all branches, local and remote
- `-r` = show remote branches only
- `-m` = rename a branch
- `-d` = delete a branch safely
- `-D` = force delete a branch
- `--show-current` = show only the current branch name

### Examples
```bash
git branch
git branch -a
git branch --show-current
```

---

<a id="git-branch-m"></a>
## git branch -m

```bash
git branch -m <new-name>
```

### What it does
Renames your current local branch.

### Syntax
```bash
git branch -m <new-name>
git branch -m <old-name> <new-name>
```

### Common options
- `-m` = rename branch
- `-M` = force rename even if the target name exists

### Examples
```bash
git branch -m feature/12345-users-dash-update
git branch -m old-name new-name
git branch -M old-name new-name
```

---

<a id="git-branch-d"></a>
## git branch -d

```bash
git branch -d <branch>
```

### What it does
Deletes a local branch if Git sees it as merged.

### Syntax
```bash
git branch -d <branch>
```

### Common options
- `-d` = delete branch safely
- `-D` = force delete branch
- `-r -d <remote-branch>` = delete a remote-tracking branch reference locally

### Examples
```bash
git branch -d release/12345-users-dash-update
git branch -r -d origin/release/12345-users-dash-update
```

---

<a id="git-branch-capital-d"></a>
## git branch -D

```bash
git branch -D <branch>
```

### What it does
Force deletes a local branch even if Git does not think it is merged.

### Syntax
```bash
git branch -D <branch>
```

### Common options
- `-D` = force delete branch

### Examples
```bash
git branch -D release/12345-users-dash-update
```

### Caution
Use this only after confirming the PR completed and the branch is no longer needed.

---

<a id="git-add"></a>
## git add

```bash
git add <path>
git add .
```

### What it does
Stages changes for commit.

### Syntax
```bash
git add <path>
git add .
```

### Common options
- `<path>` = stage a specific file or folder
- `.` = stage everything under the current folder
- `-A` or `--all` = stage all changes, including deletions
- `-u` = stage modified and deleted tracked files, but not new untracked files
- `-p` = interactively stage parts of a file

### Examples
```bash
git add "MyReport.Report" "MyReport.SemanticModel"
git add .
git add -A
git add -p
```

---

<a id="git-commit-m"></a>
## git commit -m

```bash
git commit -m "message"
```

### What it does
Creates a commit with a message describing your changes.

### Syntax
```bash
git commit -m "message"
```

### Common options
- `-m` = provide the commit message inline
- `-a` = automatically stage tracked modified/deleted files before commit
- `--amend` = change the last commit
- `--no-verify` = skip commit hooks

### Examples
```bash
git commit -m "Update dashboard layout"
git commit --amend -m "Update dashboard layout and labels"
git commit -a -m "Update tracked files"
```

---

<a id="git-push"></a>
## git push

```bash
git push
```

### What it does
Pushes your latest local commits to the linked remote branch.

### Syntax
```bash
git push
git push origin <branch>
```

### Common options
- `-u` = set upstream tracking
- `--force` = force push
- `--force-with-lease` = safer force push
- `--delete` = delete a remote branch
- `--tags` = push tags too

### Examples
```bash
git push
git push origin main
git push --force-with-lease
git push origin --delete release/12345-users-dash-update
```

---

<a id="git-push-u"></a>
## git push -u

```bash
git push -u origin <branch>
```

### What it does
Pushes your local branch to remote and links the two branches.

### Syntax
```bash
git push -u origin <branch>
```

### Common options
- `-u` = set upstream tracking
- `origin` = remote repository name
- `<branch>` = branch to push
- `--force-with-lease` = safer force push if needed

### Examples
```bash
git push -u origin feature/12345-users-dash-update
git push -u origin release/12345-users-dash-update
```

---

<a id="git-fetch"></a>
## git fetch

```bash
git fetch origin
```

### What it does
Downloads updated remote branch information without changing your local files.

### Syntax
```bash
git fetch origin
```

### Common options
- `origin` = remote repository name
- `--all` = fetch from all remotes
- `--tags` = fetch tags too
- `--prune` = remove stale remote-tracking branches

### Examples
```bash
git fetch origin
git fetch --all
git fetch --tags
```

---

<a id="git-fetch-prune"></a>
## git fetch --prune

```bash
git fetch --prune origin
```

### What it does
Refreshes remote branch information and removes references to remote branches that no longer exist.

### Syntax
```bash
git fetch --prune origin
```

### Common options
- `--prune` = remove stale remote-tracking branch references
- `origin` = remote repository name
- `--all` = prune/fetch all remotes

### Examples
```bash
git fetch --prune origin
git fetch --prune --all
```

---

<a id="git-restore-source"></a>
## git restore --source

```bash
git restore --source origin/dev <path>
```

### What it does
Copies a file or folder from another branch into your current branch.

### Syntax
```bash
git restore --source <branch> <path>
git restore --source <branch> --staged --worktree -- <path>
```

### Common options
- `--source <branch>` = restore from a specific branch or commit
- `--staged` = restore the staging area
- `--worktree` = restore the working tree files
- `--` = separate options from the file path
- `<path>` = file or folder to restore

### Examples
```bash
git restore --source origin/dev "powerbi/solutions/My Report.Report"
git restore --source origin/dev "powerbi/solutions/My Report.SemanticModel"
git restore --source origin/dev --staged --worktree -- "powerbi/solutions/My Report.Report"
```

---

<a id="git-rm-cached-r"></a>
## git rm --cached -r

```bash
git rm --cached -r <path>
```

### What it does
Stops Git from tracking a file or folder, but leaves it on your local machine.

### Syntax
```bash
git rm --cached -r <path>
```

### Common options
- `--cached` = remove from Git tracking only
- `-r` = apply recursively through a folder
- `-f` = force removal
- `<path>` = file or folder to stop tracking

### Examples
```bash
git rm --cached -r "MyReport.SemanticModel/.pbi"
git rm --cached "path/to/file.txt"
git rm --cached -r -f "path/to/folder"
```