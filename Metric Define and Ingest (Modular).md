# Entry Criteria for PBI 1: Define Metric

Before the PBI starts, all the following must be true:
- Customer owner is identified.
- Sponsor is identified.
- Metric is established or settled and on its way to becoming established.
- Metric is not a duplicate of an existing metric with a different name.
- Metric name has been agreed upon that meets agreed naming conventions.
- The PBI card for metric definition has been created.

---

# PBI 1: Define Metric

**CMMR: Metric – Define - UpdateMetricNameHere**

## PBI description
Turn this customer metric into a precise, customer-verifiable definition so the requesting customer can review and approve a definition that is ready for ingestion into the Claims Manager Metric Report.

## PBI acceptance criteria
- The metric request was reviewed and is eligible to be added to the CMMR.
- A customer-verifiable SQL definition and supporting notes are documented in the PBI discussion.
- The metric and all required reporting parameters are recorded in the maintained source used to update DimMetric, and any metric-related questions that were asked have documented customer responses.
- If revisions were requested by the customer, the requested changes are completed and clearly documented.
- The requesting sponsers have approved the metric definition.
- The PBI card for metric ingestion has been created and linked to this PBI.

---

## 01. Draft customer-verifiable metric query

### Description
Create the initial SQL definition for the requested metric using customer-accessible external tables. The SQL must be precise enough for the customer to run independently and verify that the definition matches the intended business meaning. Include supporting notes that explain the definition source, any assumptions, exclusions, and interpretation decisions made while drafting the query. The output must be grouped by the accounting month and org groups specified by the sponsor group.

While completing this task, many of the metric parameters may naturally become known. Those do not need to be finalized here, but they should inform the next task.

### Acceptance criteria
- The customer-verifiable metric query is documented in the PBI discussion.
- Supporting notes are documented in the PBI discussion.
- The definition is grouped by accounting month and org groups specified by the sponsor group.
- The definition source is identified as certified, trusted, or customer/SME-supported.
- Any assumptions, exclusions, interpretation decisions, or unresolved definition questions are explicitly documented.
- The definition is complete enough for customer review.

---

## 02. Register metric and available parameters

### Description
Once the metric definition is complete enough for review, create or update the metric record in the source used to populate DimMetric with the metric name, and metric status of 'QA', and add all other parameter values that are already known from the definition work. This task is not meant to block on every unknown value being resolved. Instead, record available information, document missing items for follow-up, and prepare those follow-ups to be requested from the customer in the next task.

This task combines internal metric registration and parameter capture so the team has one working record of what is already known before customer review begins.

### Acceptance criteria
- A metric record exists in the source used to update DimMetric.
- The metric can be uniquely identified.
- All currently known parameter values are recorded.
- Missing parameter values are documented for follow-up.
- The metric status is marked as 'QA'.
- No duplicate metric entry is created.

### Parameters to capture when available
- Metric Aliases
- CHS Flag
- Result Format
- Result YOY Change Description
- Result Inverse Flag
- Result Lag
- Result Calculation Desc
- SME
- SME Group

---

## 03. Request customer review / feedback / signoff

### Description
Notify the requesting customer that the metric definition is ready for review. Provide the SQL definition and supporting notes and ask the customer to review the query and provide feedback if needed. Include any additional follow-up questions related to the definition or missing parameter values that have been documented.

The purpose of this task is to get targeted customer input, not to make the customer re-approve information that is already complete and unambiguous.

### Acceptance criteria
- The requesting customer has been notified.
- The notification includes or links to the SQL definition and supporting notes.
- The customer is asked to review the definition query.
- Any follow-up questions or uncertainties are clearly called out.
- The communication is documented in the PBI.

---

## 04. Obtain sponsor-group signoff

### Description
If the metric applies to more than one sponsor, create a separate task for each applicable group and capture each sponsor’s review outcome. This ensures that metrics with cross-group relevance are aligned with the appropriate business stakeholders before final approval is recorded. These tasks are just place holders indicating we are waiting for sponsor signoff. During this time the PBI card should be moved to the customer signoff bucket in the board.

### Acceptance criteria
- A separate signoff task exists for each applicable sponsor.
- Each group task documents one of the following:
  - approved
  - approved with comments
  - changes requested
  - not applicable
- Required group approvals are completed before the Define Metric PBI is closed.

---

## 05. Capture customer approval or feedback

### Description
If any revisions are requested, make sure they are clearly documented, including any information about missing parameter values that was requested from the customer. If approved, confirm that all required parameter values are known and all customer-requested revisions have been addressed. Create a PBI for metric ingestion and link it to this metric definition PBI.

While defining the metric, if you did not recognize a source table, check whether it exists in the warehouse. If the table does not exist in the warehouse, create an dependency link PBI to the metric definition PBI.

### Acceptance criteria
- If revisions were requested, the requested changes are clearly documented, including any information about missing parameter values that was requested.
- The requesting customer’s approval is documented.
- All required parameter values are known before this task is completed.
- The current approved version of the SQL and notes is identifiable in the PBI.
- The PBI card for metric ingestion has been created and linked.
- Any dependency PBIs for source table ingestion that may have been identified.

---

# Entry Criteria for PBI 2: Ingest Metric

Before the PBI starts, all the following must be true:
- The Define Metric PBI has been completed and approved.
- The PBI card for metric ingestion has been created and linked to the approved Define Metric PBI.
- Any dependency PBIs, that were attached dureing the define metric steps, have been completed.
- All required metric parameters are known.

---

# PBI 2: Ingest Metric

**CMMR: Metric – Ingest - UpdateMetricNameHere**

## PBI description
Ingest the approved customer metric into the warehouse model, reconcile it at each layer, validate it to the Claims Mananger Metric report, obtain report SME approval, and activate the metric for customer consumption.

## PBI acceptance criteria
- The approved metric definition has been rebuilt using warehouse source tables and reconciled to the approved customer definition at the row level.
- An abbreviated metric extract script has been created and reconciled to the replicated query at the row level.
- A full metric extract script has been created and added to etl.MetricMartJobConfig.
- The metric has been loaded into the MetricMart using the approved stored procedure only.
- Validation to the Claims Manager Metric Report has been completed after the daily refresh cycle completed, and the aggregated abbreviated metric extract reconciliation shows zero differences.
- Report SME approval is documented in the PBI.
- The metric status has been updated to Active and is reflected in DimMetric after the normal update process.
- The metric is available in the report after the next report refresh.

---

## 01. Replicate and reconcile approved query

### Description
Rebuild the customer-approved metric definition using warehouse source tables only. If a required source table does not exist in the warehouse, create and link a dependency PBI to ingest that table before continuing and place this PBI on hold.

Reconcile the warehouse-based version back to the approved customer definition. The standard is zero differences at the row level. If differences exist due to timing or another valid reason, each difference must be documented and reviewed with the requesting customer before work continues.

### Acceptance criteria
- A warehouse-based version of the approved metric query is documented.
- The replicated query uses only warehouse source tables.
- A row-level reconciliation between the approved definition and the replicated query has been completed.
- Zero differences are confirmed, or each difference is individually documented.
- Any documented differences have been reviewed and approved by the requesting customer.
- Reconciliation evidence is attached or linked in the PBI.
- If a required source table is missing, a dependency PBI is created and linked, and this PBI is placed on hold until the dependency is complete.

---

## 02. Create and reconcile metric extract

### Description
Create the abbreviated metric extract script that includes the accounting month, reporting org, necessary business keys, numerator, and denominator. The script can only reference one fact table and the necessary dimension tables to generate these fields. Only include temp tables or CTEs if absolutely necessary; they should be the exception and not the rule.

Reconcile the abbreviated metric results back to the replicated source-table query. The standard is zero differences at the row level.

Note: Metric extracts can only use fact and dimension tables. If a required field does not exist in the referenced fact table or an available dimension table, then place this PBI on hold and create and link a dependency PBI to add the new field or fields to the fact table before continuing. On occasion, a new attribute introduced for a new metric may justify creating a dimension table. If that is the case, create and link two dependency PBIs: one to ingest the new dimension table and one to add the new dimension table’s foreign key to the referenced fact table.

### Acceptance criteria
- An abbreviated metric extract script exists for the metric.
- The script contains the accounting month, reporting org, necessary business keys, numerator, and denominator.
- The script references exactly one fact table and the necessary dimension tables.
- A row-level reconciliation between the metric extract and the replicated query has been completed.
- Zero differences are confirmed.
- Reconciliation evidence is attached or linked in the PBI.
- Any discrepancy is resolved before the metric mart is loaded.
- Any required model changes result in linked dependency PBIs.
- If dependency PBIs are required, this PBI is placed on hold until the dependencies are complete.

---

## 03. Load metric mart via approved SP

### Description
Convert the abbreviated metric extract script into the full metric extract script that includes the insert into statement, all required fields used for the metric definition and global filters, and the parameterized metric lag and date parameters.

Add the full metric extract script to the etl.MetricMartJobConfig table and load the metric into the target MetricMart using the approved stored procedure only. This approved stored procedure must be used because it handles incremental loading and safe reload behavior for high-volume data. The direct insert script must not be used to load the MetricMart due to the likelihood that it will crash the server.

### Acceptance criteria
- The full metric extract script was added to the etl.MetricMartJobConfig table.
- The approved stored procedure was used to load or reload the metric into the MetricMart.
- The load completed successfully without unresolved errors.
- The target MetricMart now contains the expected metric data.
- A full metric extract script was created and is attached or linked in the PBI.

---

## 04. Validate and reconcile metric in report

### Description
After the normal report refresh completes, export the metric results from the Power BI report. There is a hidden page in the report that can be accessed by clicking in the very lower right-hand corner of the home page. This hidden page is set up so you can filter for the new metric and export the results.

Reconcile the exported results back to the abbreviated metric extract results. The abbreviated metric extract results must be summarized by accounting month and reporting org. The standard is zero differences at the aggregated level.

### Acceptance criteria
- Validation occurred after the normal report refresh.
- The exported report results were reconciled to the abbreviated metric extract results.
- Aggregate reconciliation to the metric extract shows zero differences.
- Validation evidence is attached or linked in the PBI.

---

## 05. Obtain report SME signoff

### Description
Set up and complete a review with a report SME. The report SME reviews the customer approval, customer-facing definition, comments and discussion history, and each reconciliation performed during ingestion. Any questions raised during review must be resolved before their approval is recorded and this task is completed.

### Acceptance criteria
- A report SME review has been completed.
- The PBI documents that the report SME reviewed:
  - customer approval
  - customer-facing definition/query
  - discussion history and customer comments
  - reconciliation evidence
  - full metric extract query
- Report SME approval is documented in the PBI.
- Any issues raised during the review were resolved.

---

## 06. Set metric status to Active

### Description
update the metric status in the source used to populate DimMetric to 'Active'. Any recorded metric changes are only updated in the DimMetric table nightly and only refreshed in the report after the subsequent report refresh is complete.

This step may only occur after report SME signoff is complete.

### Acceptance criteria
- The recorded metric status has been updated to Active.
- The metric is reflected as Active in DimMetric after the normal update process.
- The status was not updated before report SME signoff was completed.
- The metric is available in the report after the next report refresh.

---

If you want, I can next format this into a version that is even easier to paste into Azure DevOps or Jira, with each PBI and task in a compact backlog-entry layout.
