# Power BI Git Workflow

## 1. Go to your repo using Pygressive
Tags: setting up Pygressive, going to Pygressive

---

## 2. Figure out where you are / were

```bash
git status
```

`[GIT-STATUS]` Shows your current branch and whether you have staged, unstaged, or untracked changes.  
Tags: how to interpret git status output

---

## 3. Pull latest version from `main`

```bash
git checkout main
git pull origin main
```

- `git checkout main` `[GIT-CHECKOUT-BRANCH]` switches you to the `main` branch
- `git pull origin main` `[GIT-PULL]` downloads and applies the latest changes from remote `main` to your local `main`

> Note: you may have to enter your password for Azure DevOps.  
> Tag: where do I find this password

---

## 4. Create a branch for your next task

```bash
git switch -c feature/WorkItemId-ReportName-Action
```

`[GIT-SWITCH-C]` Creates a **new branch** from your current branch and switches you to it.

### If the branch already exists

```bash
git checkout feature/WorkItemId-ReportName-Action
```

`[GIT-CHECKOUT-BRANCH]` Switches to an **existing branch**.

### If you mistyped the branch name, rename it

```bash
git branch -m new-branch-name
```

`[GIT-BRANCH-M]` Renames your current local branch.

---

## 5. Open Power BI report

Make sure you are working in the correct repo branch.

Make necessary changes / updates / enhancements.

---

## 6. In Pygressive, check what changed

```bash
git status
```

`[GIT-STATUS]` Confirms which files changed and whether they are tracked or untracked.

---

## 7. Stage changes for the report

```bash
git add "_solution_.Report" "_solution_.SemanticModel"
```

`[GIT-ADD-PATH]` Stages only the specific report/model folders you name.

> Note: use your exact folder names.

### If you are doing a bulk update and want all unstaged changes

```bash
git add .
```

`[GIT-ADD-ALL]` Stages all changes in the current folder and below.

---

## 8. Commit changes

```bash
git commit -m "describe changes"
```

`[GIT-COMMIT-M]` Creates a commit with a message describing your changes.

---

## 9. Push your branch to the remote repo

```bash
git branch
git push -u origin _branchname_
```

- `git branch` `[GIT-BRANCH-LIST]` shows your local branches and marks the current one with `*`
- `git push -u origin _branchname_` `[GIT-PUSH-U]` pushes your branch to remote and sets the upstream so future pushes can just use `git push`

### If the branch is already linked to remote

```bash
git push
```

`[GIT-PUSH]` Pushes your latest local commits to the already-linked remote branch.

> Note: you may have to enter your password for Azure DevOps.  
> Tag: where do I find this password

---

## 10. Go to remote repo

- Create pull request
- Complete pull request
- Review elevated changes

This PR goes from your **feature branch** into **dev**.

---

## 11. Clean up local feature branch

```bash
git fetch --prune origin
git branch
git branch -d _branchname_
```

- `git fetch --prune origin` `[GIT-FETCH-PRUNE]` refreshes remote branch info and removes deleted remote references
- `git branch` `[GIT-BRANCH-LIST]` shows local branches
- `git branch -d _branchname_` `[GIT-BRANCH-D]` deletes the local branch if Git sees it as merged

---

## 12. Get latest from remote

```bash
git fetch origin
```

`[GIT-FETCH]` Downloads the latest remote branch information without changing your working files.

---

## 13. Switch to `main` (prod)

```bash
git switch main
```

`[GIT-SWITCH]` Switches to the existing local `main` branch.

---

## 14. Update local `main` (prod)

```bash
git pull origin main
```

`[GIT-PULL]` Updates your local `main` with the latest remote prod code.

---

## 15. Create release branch from `main` (prod)

```bash
git switch -c release/WorkItemId-ReportName-Action
```

`[GIT-SWITCH-C]` Creates a new release branch from the branch you are currently on, which should be `main`.

---

## 16. Bring over the specific report from `dev`

```bash
git restore --source origin/dev "powerbi/solutions/IntelliRouter Users Dashboard.Report"
git restore --source origin/dev "powerbi/solutions/IntelliRouter Users Dashboard.SemanticModel"
```

- `git restore --source origin/dev ...` `[GIT-RESTORE-SOURCE]` copies the version of that file/folder from `origin/dev` into your current release branch

This is what lets you elevate **one report** without merging all of `dev`.

---

## 17. Review what changed

```bash
git status
```

`[GIT-STATUS]` Confirms that only the intended report/model changes are present.

---

## 18. Remove any local Power BI junk if it appeared

```bash
git rm --cached -r "powerbi/solutions/IntelliRouter Users Dashboard.SemanticModel/.pbi"
```

`[GIT-RM-CACHED-R]` Stops tracking the `.pbi` folder in Git without deleting it from your local machine.

Use this for local artifacts like:
- `.pbi/cache.abf`
- other local Power BI desktop-generated files

---

## 19. Commit the release branch

```bash
git add .
git commit -m "Release _reportname_ from dev to prod"
```

- `git add .` `[GIT-ADD-ALL]` stages all current changes
- `git commit -m ...` `[GIT-COMMIT-M]` creates the release commit

---

## 20. Push the release branch

```bash
git push origin -u release/WorkItemId-ReportName-Action
```

`[GIT-PUSH-U]` pushes the release branch to remote and links it for future pushes.

---

## 21. Go to remote repo

- Create pull request
- **Important:** the repo may default the PR target to `dev`; change it to `main`
- Complete pull request
- Go to the `Claims-BI-Prod` workspace
- Open source control
- In Updates, confirm your report is there and select **Update all**
- Review elevated changes

This PR goes from your **release branch** into **main**.

---

## 22. Refresh your local repo and remove deleted remote branches

```bash
git fetch --prune origin
```

`[GIT-FETCH-PRUNE]` updates your remote branch list and removes branches that were deleted in Azure DevOps after PR completion.

---

## 23. Switch to `main` and update it

```bash
git switch main
git pull origin main
```

- `git switch main` `[GIT-SWITCH]` switches to local `main`
- `git pull origin main` `[GIT-PULL]` brings your local `main` up to date with the completed prod PR

---

## 24. Delete your local release branch

```bash
git branch
git branch -d release/WorkItemId-ReportName-Action
```

- `git branch` `[GIT-BRANCH-LIST]` shows your local branches
- `git branch -d ...` `[GIT-BRANCH-D]` deletes the local release branch if Git sees it as merged

> Note: If Azure DevOps used a squash merge, Git may warn that the branch is not merged to `HEAD` even though the PR completed. After confirming the PR is merged and `main` has the changes, you can use:
>
> ```bash
> git branch -D release/WorkItemId-ReportName-Action
> ```
>
> `[GIT-BRANCH-D-FORCE]` force deletes the local branch.

---

## You are ready for your next project

---

# Git Command Glossary

## [GIT-STATUS] `git status`
Shows:
- your current branch
- staged changes
- unstaged changes
- untracked files

Use this anytime you want to know the current state of your working folder.

---

## [GIT-CHECKOUT-BRANCH] `git checkout <branch>`
Switches to an existing branch.

Example:

```bash
git checkout main
```

Use this if the branch already exists locally.

---

## [GIT-PULL] `git pull origin <branch>`
Gets the latest changes from the remote branch and applies them to your local branch.

Example:

```bash
git pull origin main
```

Use this to update your local copy of `main` or `dev`.

---

## [GIT-SWITCH-C] `git switch -c <new-branch>`
Creates a new branch from your current branch and switches you to it.

Example:

```bash
git switch -c feature/12345-Users-Dashboard-Update
```

Use this when starting new work or creating a release branch.

---

## [GIT-BRANCH-M] `git branch -m <new-name>`
Renames your current local branch.

Example:

```bash
git branch -m feature/12345-Users-Dash-Update
```

Use this if you created or typed the branch name incorrectly.

---

## [GIT-ADD-PATH] `git add <path>`
Stages only the specific files or folders you name.

Example:

```bash
git add "MyReport.Report" "MyReport.SemanticModel"
```

Use this when you want to control exactly what goes into the commit.

---

## [GIT-ADD-ALL] `git add .`
Stages all changed files under the current folder.

Use this when you want to include everything currently changed.

---

## [GIT-COMMIT-M] `git commit -m "message"`
Creates a commit with a commit message.

Example:

```bash
git commit -m "Update Users Dashboard visuals"
```

Use this after staging your changes.

---

## [GIT-BRANCH-LIST] `git branch`
Shows your local branches.  
The current branch has an asterisk `*`.

Use this to confirm which branch you are on.

---

## [GIT-PUSH-U] `git push -u origin <branch>`
Pushes your local branch to the remote repo and links the two branches.

Example:

```bash
git push -u origin feature/12345-Users-Dashboard-Update
```

After this, future pushes on that branch can usually be done with just `git push`.

---

## [GIT-PUSH] `git push`
Pushes your latest local commits to the linked remote branch.

Use this after the upstream has already been set.

---

## [GIT-FETCH-PRUNE] `git fetch --prune origin`
Refreshes remote branch information and removes references to remote branches that no longer exist.

Use this after PR completion and branch cleanup in Azure DevOps.

---

## [GIT-BRANCH-D] `git branch -d <branch>`
Deletes a local branch if Git sees it as merged.

Example:

```bash
git branch -d release/12345-Users-Dashboard-Update
```

Use this for normal local branch cleanup.

---

## [GIT-FETCH] `git fetch origin`
Downloads updated remote branch information without changing your local files.

Use this to refresh what your local Git knows about the remote repo.

---

## [GIT-SWITCH] `git switch <branch>`
Switches to an existing local branch.

Example:

```bash
git switch main
```

This is the newer, clearer version of using `checkout` for branch switching.

---

## [GIT-RESTORE-SOURCE] `git restore --source <branch> <path>`
Copies a file or folder from another branch into your current branch.

Example:

```bash
git restore --source origin/dev "powerbi/solutions/Users Dashboard.Report"
```

Use this when elevating one report from `dev` to a release branch off `main`.

---

## [GIT-RM-CACHED-R] `git rm --cached -r <path>`
Stops Git from tracking a file or folder, but leaves it on your local machine.

Example:

```bash
git rm --cached -r "MyReport.SemanticModel/.pbi"
```

Use this for local-only Power BI files that should not be committed.

---

## [GIT-BRANCH-D-FORCE] `git branch -D <branch>`
Force deletes a local branch even if Git does not think it is merged.

Example:

```bash
git branch -D release/12345-Users-Dashboard-Update
```

Use this only after confirming the PR completed and the branch is no longer needed. This often comes up after squash merges in Azure DevOps.