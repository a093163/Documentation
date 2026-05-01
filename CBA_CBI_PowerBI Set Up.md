# Recommended final setup

## Branches
- `main` = prod
- `dev` = development integration
- `feature/*` = all developer work

## Approval model
### Into `dev`
- source: `feature/*`
- target: `dev`
- any team member can review
- any team member can complete

### Into `main`
- source: `dev`
- target: `main`
- one of the two leads must approve
- only the two leads should complete

---

# Step-by-step instructions

---

## Step 1: Confirm the branches exist

Go to:

- `Repos`
- `Branches`

Make sure you have:
- `main`
- `dev`

If `dev` does not exist yet:
- create `dev` from `main`

---

## Step 2: Configure branch policies on `dev`

Go to:

- `Repos`
- `Branches`
- find `dev`
- click `...`
- select **Branch policies**

## Goal for `dev`
Keep it lightweight:
- PR required by upstream/company setting
- any team member can approve
- any team member can complete
- still require some review discipline

## Recommended `dev` branch policy settings

### Minimum number of reviewers
Set to:
- **1**

This supports:
- feature branch PRs must be reviewed
- but not by a lead specifically

### Enable these options
If available, turn on:

- **Reset reviewer votes when there are new changes**
- **Check for comment resolution**
- **Prohibit the most recent pusher from approving**

That gives you a sensible control:
- author raises PR
- someone else approves
- comments must be resolved

### Linked work items
Turn on if your team uses Azure Boards / tickets.

### Merge strategy
For PRs into `dev`, I recommend:

- **Squash merge** only

Why this is good for PBIP:
- feature branches may have many noisy commits
- PBIP repos can produce verbose file changes
- squash keeps `dev` history cleaner

## Do not add lead-specific rules on `dev`
Because you said that would create too much overhead.

---

## Step 3: Keep `dev` branch permissions simple

Go to:

- `Repos`
- `Branches`
- `dev`
- `...`
- **Branch security**

## Goal
Because your company setting already enforces PR-based changes, and you want any team member to complete PRs into `dev`, keep `dev` permissive.

### Recommended approach on `dev`

Normal Team:
Set:

- **Contribute** = Allow (inherited)
- **Force push** = Deny
- **Bypass policies when pushing** = Deny
- **Bypass policies when completing pull requests** = Deny
- Everything else leave as Not Set or Allow (inherited)

### For dev -> main approvers
Set:

- **Contribute** = Allow (inherited)
- **Force push** = Deny
- **Bypass policies when pushing** = Deny
- **Bypass policies when completing pull requests** = Deny

For 'Project Administrators' 

- leave as Not Set unless your admins wnat explicit rights

## Why
Your desired model for `dev` is collaborative and low overhead.

---

## Step 4: Configure branch policies on `main`

Go to:

- `Repos`
- `Branches`
- find `main`
- click `...`
- select **Branch policies**

## Goal for `main`
This is your controlled production gate:
- PR required
- one lead must approve
- only leads should complete
- release should come from `dev`

## Recommended `main` branch policy settings

### Minimum number of reviewers
Set to:
- **1**

Because you want one lead approval, not both.

### Automatically included reviewers
If your Azure DevOps UI supports this, add:
- `Authorized Approver 1`
- `Authorized Approver 2`

## Important note
Test how your tenant behaves:

- In some configurations, auto-included reviewers are just added to the PR
- In others, if marked required, both may become mandatory

Since you want **one of the two leads**, the safest pattern is usually:

- **Minimum reviewers = 1**
- add both leads as reviewers automatically if possible
- do **not** configure both as individually mandatory if that forces both approvals

### Enable these options
Turn on:

- **Reset reviewer votes when there are new changes**
- **Check for comment resolution**
- **Prohibit the most recent pusher from approving**

### Linked work items
Turn on if your team uses them.

### Merge strategy
For `dev -> main`, I recommend:

- **Merge** only

Why:
- easier to identify release/promote events
- cleaner production history for audit/tracing

---

## Step 5: Restrict who can complete PRs into `main`

Go to:

- `Repos`
- `Branches`
- `main`
- `...`
- **Branch security**

## Goal
Only the two leads should be able to complete PRs into `main`.

Because everyone is only in `Contributors`, do this by **individual user entries** on `main`.

### For each normal team member
Set on `main`:

- **Contribute** = Deny
- **Force push** = Deny
- **Bypass policies when pushing** = Deny
- **Bypass policies when completing pull requests** = Deny

### For dev -> main approvers
Set:

- **Contribute** = Allow
- **Force push** = Deny
- **Bypass policies when pushing** = Deny
- **Bypass policies when completing pull requests** = Deny

## Optional: your own account
Only give yourself `Allow` on `main` if you genuinely need emergency fallback release access.

Otherwise leave yourself out to keep production tighter.

---

## Step 6: Define the branch naming convention

You likely can’t enforce naming technically, so standardize it by team agreement.

## Recommended convention
- `feature/WorkOrder#-Subject-Action`

## Pattern
`<feature>/<workitem#>-<short-description>-<action>`

This works well for PBIP repos because it helps reviewers understand intent quickly.

---

## Step 7: Define the expected PR flow

Because part of your model is process-driven, make it explicit.

## Feature development
- Branch from `dev`
- Work in `feature/*`
- PR into `dev`

### Example
- source: `feature/PBI-101-add-exec-summary`
- target: `dev`

## Production promotion
- PR from `dev`
- target `main`

### Example
- source: `dev`
- target: `main`

## Team rule
Document:

> Developers must not open PRs from feature branches directly to `main`.  
> Production promotion happens from `dev` to `main` only.

---

## Step 8: How to handle “only dev can merge into main”

With your current access, this is still mostly a **process rule**, unless some upstream policy already validates source branch.

So operationally:

- only leads can complete PRs into `main`
- leads approve only `dev -> main`
- reject:
  - `feature/* -> main`
  - `bugfix/* -> main`
  - `chore/* -> main`

That is usually good enough in practice when the number of `main` completers is just two.

---

# Recommended day-to-day workflow

## Developer workflow
1. Pull latest `dev`
2. Create feature branch from `dev`

Example:
- `feature/PBI-123-add-margin-analysis`

3. Make PBIP changes
4. Commit and push
5. Open PR:
   - source = feature branch
   - target = `dev`
6. Another team member reviews/approves
7. Any team member completes the PR into `dev`

## Lead/release workflow
1. Open PR:
   - source = `dev`
   - target = `main`
2. One lead reviews/approves
3. One lead completes the PR into `main`

---

# Recommended settings summary

## `dev`
### Branch security
- keep permissive enough for team PR completion
- no special lead-only control

### Branch policies
- Minimum reviewers = 1
- Reset votes on new changes = On
- Comment resolution required = On
- Most recent pusher cannot approve = On
- Linked work items = Optional
- Merge type = Squash

## `main`
### Branch security
For each normal team member:
- Contribute = Deny
- Force push = Deny
- Bypass push policies = Deny
- Bypass PR policies = Deny

For each lead:
- Contribute = Allow
- Force push = Deny
- Bypass push policies = Deny
- Bypass PR policies = Deny

### Branch policies
- Minimum reviewers = 1
- Add both leads as reviewers if possible
- Reset votes on new changes = On
- Comment resolution required = On
- Most recent pusher cannot approve = On
- Linked work items = Optional
- Merge type = Merge

---

# Common pitfalls to avoid

## 1. Don’t deny `Contributors` on `main`
Since your leads are also contributors, that would block them too.

Use **individual users** on `main`.

## 2. Don’t over-engineer `dev`
You’ve already decided `dev` should be lightweight. Good choice.

## 3. Be careful with auto-included reviewers on `main`
Test whether adding both leads makes both mandatory. If yes, rely on:
- minimum reviewers = 1
- and lead-only completion rights

## 4. Don’t assume branch policy alone enforces source branch
You still need the team rule:
- only `dev` goes to `main`

---

# PBIP-specific best practices

## Keep PRs small
PBIP file diffs can become noisy quickly.

## Require screenshots in PR descriptions
For report changes, ask for:
- page changed
- screenshot
- semantic model impact
- DAX changes
- testing notes

## Standardize Power BI Desktop version
This reduces unnecessary metadata churn.

## Use a `.gitignore`
At minimum:

```gitignore
**/.pbi/localSettings.json
.DS_Store
Thumbs.db
```

---

# Final practical recommendation

Given your constraints, this is the best-fit model:

- `dev` = PR-based, lightweight, team-collaborative
- `main` = tightly controlled, lead-gated
- leads only required for `dev -> main`
- feature PRs into `dev` stay fast and low overhead

If you want, I can turn this into a **click-by-click checklist** with exact menu paths and what to click on each screen.
