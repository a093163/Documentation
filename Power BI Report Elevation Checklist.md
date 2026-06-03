# Power BI Source Control Checklist

## For Analysts Making Report Changes

## What This Means for You

If you are used to making report changes in Power BI Desktop and publishing directly to Dev or Prod, this new process will feel different at first.

The goal is to make your work:

- safer
- easier to track
- easier to review
- easier to move from Dev to Prod without surprises

You are still doing the same core work you do today:

- opening a report
- making changes
- saving your work
- getting it into Dev
- moving approved changes into Prod

What is changing is **how** those updates are saved, reviewed, and promoted.

Instead of publishing directly from Power BI Desktop to a workspace, you will now:

- start from the repository
- work in your own branch
- save your changes locally
- commit and push your changes to Azure DevOps
- create a Pull Request for review
- let Dev and Prod workspaces pull approved changes from source control

> **The biggest shift to keep in mind:**  
> The repository is now the source of truth, not the workspace.

## Your New Routine

1. Get the latest version of the report from Git
2. Create your own working branch
3. Make your changes locally in Power BI Desktop
4. Save, commit, and push your changes
5. Create a PR into `dev`
6. Validate the report in Dev
7. Promote only that approved report into Prod using a release branch

---

# Report Change Checklist

Use this checklist each time you make a change to a Power BI report.

---

## Part 1 — Start From the Latest Dev

### Prepare your local repository

- [ ] Open the repository in VS Code or Visual Studio
- [ ] Open a terminal in the repository folder
- [ ] Check your current branch and file status by running:

```bash
git status
```

- [ ] Confirm you are in the correct repo before continuing

### If you already have local changes

- [ ] Review the output of `git status`
- [ ] If the changes belong to another task and you are not ready to commit them, stash them:

```bash
git stash
```

- [ ] If you need those stashed changes back later, restore them:

```bash
git stash pop
```

- [ ] If you are not sure what the changed files are, stop and ask before continuing

### Get the current Dev version

- [ ] Switch to the `dev` branch:

```bash
git checkout dev
```

- [ ] Pull the latest version from Azure DevOps:

```bash
git pull origin dev
```

### Create your working branch

- [ ] Create a feature branch for your change

Example:

```bash
git checkout -b feature/1234567-claims-manager-metric-report
```

- [ ] If the branch already exists, switch to it instead

Example:

```bash
git checkout feature/1234567-claims-manager-metric-report
```

- [ ] If your feature branch already existed before today and `dev` has changed since then, update your branch with the latest `dev`

Run these commands in order:

```bash
git checkout dev
git pull origin dev
git checkout feature/1234567-claims-manager-metric-report
git merge dev
```

---

## Part 2 — Make Your Report Change Locally

### Open and update the report

- [ ] Open the report from your local repository copy
- [ ] Do not open an old PBIX from your desktop or downloads folder
- [ ] Make your changes in Power BI Desktop
- [ ] Save your changes
- [ ] Close Power BI Desktop when finished

### Typical changes may include

- [ ] Visual updates
- [ ] Filter changes
- [ ] Formatting changes
- [ ] DAX updates
- [ ] Semantic model changes, if needed

---

## Part 3 — Review, Commit, and Push Your Change

### Review your changed files

- [ ] Run:

```bash
git status
```

- [ ] Optionally list only the changed file names:

```bash
git diff --name-only
```

- [ ] Confirm the changed files match the report and semantic model you intended to update

### Stage your changes

- [ ] If all changed files belong to this report update, stage everything:

```bash
git add .
```

- [ ] If you want to be more careful, stage only the report and semantic model folders for this report

Example:

```bash
git add "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
```

### Commit your changes

- [ ] Commit using a clear message that includes the work item number

Example:

```bash
git commit -m "1234567 Update Claims Manager Metric Report filters and layout"
```

### Good commit message examples

- `1234567 Update Claims Manager Metric Report filters and layout`
- `1234567 Add trend visual to Claims Manager Metric Report`
- `1234567 Fix formatting in Claims Manager Metric Report`

### Push your branch

- [ ] Push your feature branch to Azure DevOps

Example:

```bash
git push -u origin feature/1234567-claims-manager-metric-report
```

---

## Part 4 — Create and Complete the PR Into Dev

### Create the Pull Request

- [ ] Go to Azure DevOps
- [ ] Create a Pull Request from your feature branch into `dev`
- [ ] Set **Source branch** to: `feature/1234567-claims-manager-metric-report`
- [ ] Set **Target branch** to: `dev`
- [ ] Use a clear PR title

Example title:

`1234567 Claims Manager Metric Report updates`

### Respond to review comments if needed

- [ ] Review comments from approvers
- [ ] Make requested updates locally in Power BI Desktop
- [ ] Save and close Power BI Desktop
- [ ] Stage the updated files
- [ ] Commit the updates
- [ ] Push the branch again

Example:

```bash
git add .
git commit -m "1234567 Address PR comments for Claims Manager Metric Report"
git push
```

### If your feature branch is behind `dev`

- [ ] Update your branch with the latest `dev` before completing the PR

Run these commands in order:

```bash
git checkout dev
git pull origin dev
git checkout feature/1234567-claims-manager-metric-report
git merge dev
```

- [ ] Resolve any conflicts if prompted
- [ ] Commit the merge if needed
- [ ] Push again

### Complete the Pull Request

- [ ] Confirm required reviewers have approved
- [ ] Complete the Pull Request into `dev`
- [ ] Do not push directly into `dev`

---

## Part 5 — Apply and Test in the Dev Workspace

### Sync the Dev workspace

- [ ] Open the Dev Power BI workspace
- [ ] Go to **Source Control**
- [ ] Go to **Updates**
- [ ] Refresh or check for updates
- [ ] Apply changes

### Validate the report in Dev

- [ ] Open the report in the Dev workspace
- [ ] Confirm visuals load correctly
- [ ] Confirm filters work correctly
- [ ] Confirm numbers look correct
- [ ] Confirm no unrelated changes came through

> Do not move to Prod until the report is validated in Dev.

---

## Part 6 — Promote Only That Report to Prod

### Start from `main`

- [ ] Switch to `main`:

```bash
git checkout main
```

- [ ] Pull the latest production version:

```bash
git pull origin main
```

### Create the release branch

- [ ] Create a release branch for this specific Prod promotion

Example:

```bash
git checkout -b release/1234567-claims-manager-metric-report
```

### Refresh remote branch information

- [ ] Refresh your remote branch references:

```bash
git fetch origin
```

### Replace only the approved report and semantic model

- [ ] Remove the current version of the report and semantic model from the release branch

Example:

```bash
git rm -r --ignore-unmatch "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
```

- [ ] Bring in only the approved version from `origin/dev`

Example:

```bash
git checkout origin/dev -- "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
```

### Review the release contents

- [ ] Run:

```bash
git status
```

- [ ] Run:

```bash
git diff --name-only HEAD
```

- [ ] Confirm only the intended report and semantic model are included

### Commit and push the release branch

- [ ] Stage only the intended report and semantic model

Example:

```bash
git add "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
```

- [ ] Commit the promotion

Example:

```bash
git commit -m "1234567 Promote Claims Manager Metric Report from dev to prod"
```

- [ ] Push the release branch

Example:

```bash
git push -u origin release/1234567-claims-manager-metric-report
```

---

## Part 7 — Create and Complete the PR Into Main

### Create the Prod Pull Request

- [ ] Go to Azure DevOps
- [ ] Create a Pull Request from the release branch into `main`
- [ ] Set **Source branch** to: `release/1234567-claims-manager-metric-report`
- [ ] Set **Target branch** to: `main`

Suggested PR title:

`1234567 Promote Claims Manager Metric Report to Prod`

### Complete the Pull Request

- [ ] Confirm reviewers approve the promotion
- [ ] Complete the Pull Request into `main`
- [ ] Do not push directly into `main`

---

## Part 8 — Apply and Test in the Prod Workspace

### Sync the Prod workspace

- [ ] Open the Prod Power BI workspace
- [ ] Go to **Source Control**
- [ ] Go to **Updates**
- [ ] Refresh or check for updates
- [ ] Apply changes

### Validate the report in Prod

- [ ] Open the report in Prod
- [ ] Confirm the report loads correctly
- [ ] Confirm visuals and filters work correctly
- [ ] Confirm values look correct
- [ ] Confirm only the intended report changed
- [ ] The report is now live in Prod

---

# Quick Command Reference

## Feature branch workflow for a normal report change

```bash
git status
git checkout dev
git pull origin dev
git checkout -b feature/1234567-claims-manager-metric-report
git status
git diff --name-only
git add .
git commit -m "1234567 Update Claims Manager Metric Report filters and layout"
git push -u origin feature/1234567-claims-manager-metric-report
```

## Release branch workflow for promoting one report to Prod

```bash
git checkout main
git pull origin main
git checkout -b release/1234567-claims-manager-metric-report
git fetch origin
git rm -r --ignore-unmatch "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
git checkout origin/dev -- "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
git status
git diff --name-only HEAD
git add "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
git commit -m "1234567 Promote Claims Manager Metric Report from dev to prod"
git push -u origin release/1234567-claims-manager-metric-report
```

---

# Glossary of Common Git Commands

## `git status`

**What it does:**  
Shows your current branch and any changed files.

**When to use it:**  
Before switching branches, before committing, and after making changes.

**Why it matters:**  
It helps you confirm where you are and what is pending.

---

## `git checkout`

**What it does:**  
Switches you to another branch. If used with `-b`, it creates a new branch and switches to it.

**When to use it:**  
When moving between `dev`, `main`, feature branches, and release branches.

**Why it matters:**  
If you are on the wrong branch, your work goes to the wrong place.

**Examples:**

```bash
git checkout dev
git checkout main
git checkout -b feature/1234567-claims-manager-metric-report
```

---

## `git pull`

**What it does:**  
Downloads and applies the latest changes from the remote repository to your current branch.

**When to use it:**  
Before starting work on `dev` or `main`, and anytime you need the latest changes.

**Why it matters:**  
It keeps your local copy current and reduces conflicts.

**Examples:**

```bash
git pull origin dev
git pull origin main
```

---

## `git fetch origin`

**What it does:**  
Refreshes your view of the remote branches without merging them into your current branch.

**When to use it:**  
Before bringing files from `origin/dev` into a release branch.

**Why it matters:**  
It makes sure your remote branch references are current.

**Example:**

```bash
git fetch origin
```

---

## `git merge`

**What it does:**  
Combines changes from one branch into your current branch.

**When to use it:**  
When your feature branch needs the latest `dev`.

**Why it matters:**  
It helps you stay current and resolve conflicts before PR completion.

**Example:**

```bash
git merge dev
```

---

## `git stash`

**What it does:**  
Temporarily saves uncommitted local changes and clears your working folder.

**When to use it:**  
Only if you need to switch branches but are not ready to commit your current work.

**Why it matters:**  
It prevents branch-switching problems.

**Examples:**

```bash
git stash
git stash pop
```

---

## `git add`

**What it does:**  
Stages files for the next commit.

**When to use it:**  
After reviewing your changes and before `git commit`.

**Why it matters:**  
It tells Git which changes should be included in the commit.

**Examples:**

```bash
git add .
git add "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
```

---

## `git commit`

**What it does:**  
Saves a checkpoint of your staged changes in Git history.

**When to use it:**  
After staging the intended files.

**Why it matters:**  
It creates a record of what changed and why.

**Example:**

```bash
git commit -m "1234567 Update Claims Manager Metric Report filters and layout"
```

---

## `git push`

**What it does:**  
Sends your local commits to Azure DevOps.

**When to use it:**  
After committing.

**Why it matters:**  
It publishes your branch so a Pull Request can be created.

**Examples:**

```bash
git push
git push -u origin feature/1234567-claims-manager-metric-report
```

---

## `git rm -r --ignore-unmatch`

**What it does:**  
Removes files or folders from Git tracking in the current branch.

**When to use it:**  
During a release branch when replacing the current Prod copy of a report or semantic model with the approved Dev version.

**Why it matters:**  
It clears the current production version so the correct one from `dev` can be brought in cleanly.

**Example:**

```bash
git rm -r --ignore-unmatch "Claims Manager Metric Report.Report" "Claims Manager Metric Report.SemanticModel"
```

---

## `git diff --name-only`

**What it does:**  
Lists the names of changed files.

**When to use it:**  
Before staging or before committing.

**Why it matters:**  
It helps confirm that only the expected files are included.

**Examples:**

```bash
git diff --name-only
git diff --name-only HEAD
```

---

# Team Rules

## Always do this

- Start from the latest `dev`
- Create a feature branch for every work item
- Use PRs into `dev` and `main`
- Validate in Dev before promoting to Prod
- Use a release branch to promote only the approved report

## Never do this

- Do not publish directly from Power BI Desktop to Dev or Prod
- Do not commit directly to `dev`
- Do not commit directly to `main`
- Do not promote all of `dev` to Prod unless that is intentional

If you want, I can also turn this into a more polished `README.md` with:
- a table of contents
- collapsible sections
- callout formatting
- a shorter “quick start” version for analysts