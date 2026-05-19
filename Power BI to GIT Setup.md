Absolutely. The cleanest way to think about this is:
•	Git becomes the source of truth.
•	main represents what is in Production.
•	develop represents what is in Dev.
•	Power BI workspaces do not become places where people “just publish stuff.”
•	Deployment is the act of merging branches and then syncing the target workspace from that branch.
Here’s a step-by-step way to redesign your process.
________________________________________
1) Target operating model
Use this as your end state:
Component	Purpose	Git branch
Dev workspace	Integration/testing workspace for in-progress approved changes	develop
Prod workspace	Live production content only	main
Repo CLA_CBI_PowerBI	System of record for Power BI artifacts	all branches
Recommended branch model:
•	main = exactly what is in prod
•	develop = exactly what is in dev
•	feature/* = individual work items
•	hotfix/* = urgent production fixes
•	optional release/* = if you later want a formal UAT/release branch
Example branch names:
•	feature/add-claims-aging-page
•	feature/update-rls-for-supervisors
•	hotfix/fix-prod-refresh-failure

# 2) First principle: do not leave dev and prod on the same branch

This is the most important design choice.

If your current dev workspace is syncing to the repo and that branch currently reflects dev content, that branch should **not** be your long-term `main` if `main` is supposed to represent prod.

Instead:

- Dev workspace should sync to `develop`
- Prod workspace should sync to `main`

So if your current repo state reflects dev, your next step is to **separate the branches and re-baseline `main` to prod**.
3) What to do right now, step by step
Phase A — Put controls in place before changing anything
Step 1: Freeze direct publishing to prod
Before you wire prod to Git, stop unmanaged publishing.
Do this now:
•	Remove broad Contributor/Member access from the prod workspace
•	Keep only a small release/admin group able to manage prod
•	Tell report authors: “No direct PBIX publishing to prod starting now”
Recommended access:
•	Dev workspace: 
o	BI developers = Contributor/Member
•	Prod workspace: 
o	BI platform admins/release managers = Admin/Member
o	report consumers = App audience / Viewer
o	report developers = not Contributors in prod unless absolutely necessary
Step 2: Document the current state
Capture:
•	reports
•	semantic models/datasets
•	dataflows
•	refresh schedules
•	gateway bindings
•	parameters
•	RLS roles
•	app setup
•	workspace permissions
This matters because some of these operational settings are not always fully managed by Git.
Step 3: Confirm which artifacts are Git-compatible
If you have older or unsupported items, note them now.
Important reality: Git sync covers the Power BI/Fabric item definitions, but things like:
•	credentials
•	gateway connections
•	some refresh settings
•	some access/app settings
may still need separate operational handling.
So your future process should be “Git-driven for content,” with a short operational checklist for environment-specific settings.
________________________________________
Phase B — Re-baseline your repo correctly
This is the key cutover step.
Step 4: Create a develop branch from the current dev-synced state
If the branch currently synced from dev contains your latest development content, preserve it as develop.
If your dev workspace is currently syncing to main, do this conceptually:
•	create develop from the current branch state
•	repoint the dev workspace to develop
Result:
•	develop = current dev state
•	main is now free to become the true prod baseline
Step 5: Repoint the dev workspace to develop
In the workspace Git settings:
•	Repo: CLA_CBI_PowerBI
•	Branch: develop
•	Folder: use the same folder path you intend to keep long term
From this point forward, your dev workspace should stay on develop.
Do not keep flipping the dev workspace between branches for normal work.
________________________________________
Phase C — Make main represent actual production
This is the part you asked about specifically.
Step 6: Create a temporary branch for the current prod baseline
Create a temporary branch, for example:
•	prod-baseline
Step 7: Connect the prod workspace to prod-baseline
In the prod workspace Git settings:
•	Repo: CLA_CBI_PowerBI
•	Branch: prod-baseline
•	Folder: same folder structure as dev
Then commit the current production workspace state to Git.
This gives you a Git snapshot of what is really live today.
Step 8: Review the differences between develop and prod-baseline
This will show you the gap between:
•	what developers have in dev
•	what users currently have in prod
That gap is normal.
Step 9: Make main equal the prod baseline
Now set up main so that it represents production.
You can do that by either:
•	merging prod-baseline into main, or
•	replacing/recreating main so it matches prod-baseline
The goal is simple:
main = current prod
Not “future desired state.” Not “whatever dev has.” Only actual production.
Step 10: Reconnect the prod workspace from prod-baseline to main
Once main is aligned to production, point the prod workspace to:
•	Repo: CLA_CBI_PowerBI
•	Branch: main
After that, the prod workspace should normally only pull from main.
Best practice:
•	Prod workspace should almost never commit back to Git
•	Exception: initial baseline or emergency recovery only
So your stable mapping becomes:
•	Dev workspace → develop
•	Prod workspace → main
That is the core design.
________________________________________
4) Your ongoing workflow
Now let’s define the day-to-day operating model.
________________________________________
Standard workflow for normal changes
Step 1: Create a work item
Every change starts with a request or ticket.
Example:
•	“Add Claims Aging page to Executive Dashboard”
•	“Update RLS so regional managers only see assigned claims”
Step 2: Create a feature branch from develop
Examples:
•	feature/add-claims-aging-page
•	feature/update-rls-regional-managers
Why branch from develop? Because develop is your shared next-version integration branch.
Step 3: Make changes locally, not by publishing directly to the workspace
This is a major maturity improvement.
Recommended developer approach:
•	clone the repo locally
•	work in Power BI Desktop using PBIP/project-based files where possible
•	commit changes to the feature branch
If you still have PBIX-heavy development today, transition toward PBIP as your authoring model. It is much better for version control.
Best practice:
•	Developers should not treat the shared dev workspace as their personal scratchpad
•	Use local development and feature branches first
•	Use the shared dev workspace for integrated testing after merge to develop
Step 4: Open a pull request from feature/* into develop
Require at least:
•	1 reviewer from the BI team
•	no direct pushes to develop
Suggested review checklist:
•	naming standards followed
•	model changes documented
•	RLS tested
•	refresh tested
•	visuals validated
•	no unintended object deletions
Step 5: Merge to develop
After approval, merge the PR.
Then update the dev workspace from Git so the dev workspace matches develop.
This is your integration deployment.
Step 6: Test in the dev workspace
Use dev as your pre-prod validation environment.
Validate:
•	data refresh
•	report rendering
•	drillthrough / bookmarks / filters
•	RLS
•	gateway connectivity
•	performance
•	app behavior if applicable
Step 7: Release to prod through a PR from develop to main
When the dev version is approved for release:
•	open PR: develop → main
This PR is the release candidate.
Step 8: Require stronger approval for main
For main, I recommend:
•	2 approvers minimum
•	one BI/platform approver
•	one business/data owner approver
•	no self-approval
•	no direct pushes
•	require linked work item/change record if your org uses them
Step 9: Merge to main
Once approved, merge the release PR.
Step 10: Sync the prod workspace from main
Now update the prod workspace from Git.
This is your production deployment step.
Step 11: Run post-deployment validation
After prod sync:
•	confirm semantic model refresh
•	validate key reports
•	validate app audience visibility
•	validate RLS for at least 1–2 test users
•	confirm no gateway/credential issues
Step 12: Tag the release
Example tags:
•	powerbi-prod-2026-05-19
•	v2026.05.19
This gives you a rollback reference.
________________________________________
5) Recommended branch and approval rules
Branches
main
Purpose:
•	production only
Rules:
•	protected branch
•	no direct push
•	PR required
•	2 approvals required
•	release manager or BI lead required
•	business owner approval recommended
•	tag every release
develop
Purpose:
•	integrated dev state
Rules:
•	protected branch
•	no direct push
•	PR required
•	1 approval required
•	automated checks if you have them
feature/*
Purpose:
•	isolated work
Rules:
•	created from develop
•	short-lived
•	deleted after merge
hotfix/*
Purpose:
•	urgent prod fixes
Rules:
•	created from main
•	merged to main
•	then back-merged into develop
That last part is critical so dev doesn’t fall behind prod fixes.
________________________________________
6) Concrete example of the workflow
Let’s say your team needs to add a new “Claims Aging” report page.
Example flow
Development
1.	Create branch:
o	feature/add-claims-aging-page
2.	Developer updates:
o	report layout
o	DAX measures
o	maybe model relationship or parameter
3.	Commit changes with comments like:
o	Add Claims Aging report page
o	Create Aging Bucket measure
o	Update tooltip page for claims detail
4.	Open PR:
o	feature/add-claims-aging-page → develop
5.	Reviewer checks:
o	visuals render correctly
o	no accidental dataset renames
o	measure names follow standards
6.	Merge to develop
7.	Dev workspace syncs from develop
8.	Business users test in dev
Release
9.	Open release PR:
o	develop → main
10.	Approvals:
•	BI lead approves
•	Claims business owner approves
11.	Merge to main
12.	Prod workspace syncs from main
13.	Validate live app/report
14.	Tag release:
•	v2026.06.01
________________________________________
7) Hotfix process
Production issues should not be fixed by publishing directly to prod.
Use this pattern instead:
Example
A production report breaks because a measure is incorrect.
1.	Create branch from main:
o	hotfix/fix-claims-aging-measure
2.	Make the fix
3.	PR:
o	hotfix/fix-claims-aging-measure → main
4.	Fast approval path:
o	BI lead + one business approver
5.	Merge to main
6.	Sync prod workspace from main
7.	Back-merge main into develop
o	or open PR main → develop
This prevents the hotfix from being lost later.
________________________________________
8) Recommended workspace behavior rules
Dev workspace
Allowed:
•	integration testing
•	refresh validation
•	developer collaboration
Not ideal:
•	untracked ad hoc edits with no PR
•	using it as a personal sandbox
Prod workspace
Allowed:
•	update from Git
•	operational settings management
•	refresh monitoring
Not allowed:
•	direct PBIX publishing
•	emergency “just fix it in prod” edits
•	developers editing prod content directly
Rule of thumb: If a workspace change is not represented in Git, it should be treated as drift and corrected.
________________________________________
9) What to do with environment-specific settings
This is where many Power BI Git-driven designs fail if they don’t plan ahead.
Git should store your content definitions, but you still need an environment strategy for things like:
•	server/database parameters
•	gateway mappings
•	credentials/secrets
•	refresh schedules
•	app audiences
Recommended pattern:
For data source differences
Use parameters such as:
•	EnvironmentName
•	SqlServerName
•	DatabaseName
•	LakehouseName
Example:
•	Dev uses sql-dev.company.org
•	Prod uses sql-prod.company.org
Do not hardcode dev-only sources in content intended for prod.
For credentials
Keep them out of Git. Manage them in the service/admin process.
For refresh schedules
Treat them as environment operations:
•	documented in release checklist
•	set once, then monitored
For gateways
Document the expected gateway/data source mapping per workspace.
________________________________________
10) Recommended release checklist
Use a lightweight checklist for every release to main.
Before merge to main:
•	PR approved
•	dev refresh successful
•	business validation completed
•	impact noted
•	rollback plan identified
After prod sync:
•	prod semantic model refresh triggered or verified
•	report smoke test completed
•	RLS validated
•	app updated/verified if needed
•	release tag created
________________________________________
11) Optional but strongly recommended improvements
Add a Test/UAT workspace later
If your process matures, move to 3 environments:
•	Dev → develop
•	Test/UAT → release/*
•	Prod → main
This is especially useful if business users need formal acceptance testing.
Use PBIP for local development
This is the best long-term move if you want Git to be meaningful.
Instead of:
•	editing PBIX and publishing directly
move toward:
•	repo clone
•	PBIP project files
•	pull request review
•	workspace sync as deployment
Add automated validation
If you have the capability, add CI checks for:
•	naming standards
•	model best practices
•	banned connection strings
•	accidental deletions
________________________________________
12) My recommended final design for your specific situation
Given your current state, I would implement this exact model:
Repository
CLA_CBI_PowerBI
Branches
•	main = prod
•	develop = dev
•	feature/* = change branches
•	hotfix/* = urgent production fixes
Workspace mapping
•	Dev workspace → develop
•	Prod workspace → main
Deployment rule
•	Dev changes go to develop
•	Release happens by PR develop → main
•	Prod workspace is updated from main
Permission rule
•	No direct publish to prod
•	No direct push to main
•	PR approvals required for both develop and main
Source-of-truth rule
•	Git is authoritative
•	Workspaces are deployed environments, not unmanaged authoring surfaces
________________________________________
13) The one-time cutover sequence I would use
If I were guiding your team, I would do it in this order:
1.	Freeze direct prod publishing
2.	Snapshot/document current prod and dev
3.	Create develop from the current dev-synced branch
4.	Point dev workspace to develop
5.	Create prod-baseline
6.	Connect prod workspace to prod-baseline
7.	Commit current prod workspace state to Git
8.	Make main match prod-baseline
9.	Point prod workspace to main
10.	Protect main and develop
11.	Start using feature branches + PRs
12.	Release only through develop → main → prod sync
________________________________________
If you want, I can next turn this into one of these formats:
1.	a visual workflow diagram
2.	a SOP/runbook your team can follow
3.	a branch policy and approval template for GitHub/Azure DevOps
4.	a Power BI release checklist you can copy into Teams/SharePoint/Jira

