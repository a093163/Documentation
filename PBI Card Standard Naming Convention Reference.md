
## How do I title a PBI card?

Separate these ideas cleanly:

- Project
- Work type
- Primary target
- Requested action

## Recommended convention

`Project – Work Type – Target – Action`

This is the version I’d recommend as your default.

### Definitions

#### 1) Project
The system, initiative, or business area.

Examples:
- CMMR
- Finance
- SalesOps

#### 2) Work Type
Why the card exists.

Suggested controlled list:
- Bug
- Change
- Enhancement
- Spike
- Tech Debt
- Docs
- Data Fix
- Maintenance
- Support

You may not need all of these, but “Enhancement” or “Change” is the key missing category in your current model.

#### 3) Target
The primary thing being changed, documented, investigated, or impacted.

This can be:
- a technical asset: Table, SSIS Package, Stored Procedure, Power BI Visual
- a process/domain: Metric Ingestion, Refresh Process, Deployment Runbook
- a deliverable area: Source Mapping, Report Logic, Data Pipeline

This is the field that makes the convention flexible.

#### 4) Action
A short verb-led statement describing the requested work.

Good verbs:
- Create
- Update
- Fix
- Reload
- Add
- Remove
- Investigate
- Refactor
- Document
- Validate
- Standardize

Keep it outcome-focused and short.
