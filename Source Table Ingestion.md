# PBI

## Title
CMMR: Feature – Table – Ingest the source table [Table Name]

## Description
Ingest the external table into the CMMR metric data warehouse. Identify the source system and confirm access. Create the land table and source table DDL. Build the initial extract script and land-to-source stored procedure. Pipeline elevate the required database objects. Run and test the initial source load in production. Configure job control, add the SSIS container, configure weekly reconciliation, and obtain warehouse SME signoff.

## Acceptance Criteria
- The external table, source system, server, and database are identified in the PBI.
- Access to the external table is confirmed.
- Naming for the land table, source table, stored procedure, SSIS container, and related artifacts follows current naming conventions.
- The load approach is identified as full or incremental.
- The initial extract script is created and used to load development data to the land table.
- The land table DDL is created and pipeline elevated.
- The source table DDL is created and pipeline elevated.
- The source table includes required indexing, rowversion, required date fields, and surrogate key handling where needed.
- The land-to-source stored procedure is created, pipeline elevated, and tested in production.
- The initial source load completes successfully.
- Job control is configured for the table load.
- The SSIS solution is updated to include the source table container and is pipeline elevated.
- Weekly reconciliation is configured in Environmental Health.
- Warehouse SME signoff is documented in the PBI.

# Tasks

## 01. Identify source and load approach

### Description
Identify the external table to ingest. Record the source system, server, and database in the PBI. Confirm that the required entitlements are in place to access the table. Establish the standard names for the land table, source table, stored procedure, SSIS container, and related artifacts. Identify whether the table can be incrementally loaded. Determine if the table is larger than 10 million records and needs to use a sample extract instead of a full extract, and record that in the PBI.

### Acceptance Criteria
- The external table is identified in the PBI.
- The source system, server, and database are identified in the PBI.
- Access to the external table is confirmed.
- Standard names for the land table, source table, stored procedure, SSIS container, and related artifacts are recorded.
- The load type is identified as full or incremental.
- The need for a 10 million record sample instead of a full-table extract is identified and recorded when applicable.

---

## 02. Create initial load script

### Description
Create the initial extract script to load data from the external table into the land table. If the table is large, limit the extract to the approved development sample. If the table uses an incremental load, include the incremental filter in the extract logic.

### Acceptance Criteria
- The initial extract script is created.
- The script matches the identified load type.
- The script includes the approved development sample logic when applicable.
- The script includes the incremental filter when applicable.
- The script is attached or linked in the PBI.

---

## 03. Create and load land table in dev

### Description
Create the land table DDL on `ClaimsBIStage` using the standard naming convention. Duplicate the external table structure as closely as possible. Create the primary index on the fields used to join the land table to the source table. Run the initial extract script and load the land table in development. Load the full extract or approved development sample based on the identified load approach. Review the load results and document any issues found.

### Acceptance Criteria
- The land table DDL is created.
- The land table name follows the naming convention.
- The land table is created on `ClaimsBIStage`.
- The schema duplicates the external table structure as closely as possible.
- The primary index is created on the fields used to join the land table to the source table.
- The land table is loaded in development.
- The load completes without unresolved errors.
- The loaded data matches the expected extract scope.
- Any load issues are documented in the PBI.
- The DDL script is attached or linked in the PBI.

---

## 04. Create source table DDL

### Description
Create the source table DDL in `ClaimsBIMetrics.Source` using the standard naming convention. Match the land table structure and add the warehouse-required fields. Add a rowversion column. Add the required date fields based on whether the table is regular or temporal. If the external table does not have a stable key, create an internal surrogate key. Create the required clustered and nonclustered indexes.

### Acceptance Criteria
- The source table DDL is created.
- The source table name follows the naming convention.
- The source table is created in `ClaimsBIMetrics.Source`.
- The source table includes a rowversion column.
- The required date fields are included.
- A surrogate key is created when no stable key exists.
- Required clustered and nonclustered indexes are created.
- The rationale for regular versus temporal table design is documented in the PBI.
- The DDL script is attached or linked in the PBI.

---

## 05. Create land-to-source SP

### Description
Create the stored procedure that moves data from the land table to the source table. Include the standard script header. Use merge logic that updates rows only when a value changes. If the table uses a full load, include delete logic for records no longer in the source extract. If the table uses an incremental load, do not include source-missing deletes. Run the script and test the successful merge of data from the land table to the source table.

### Acceptance Criteria
- The stored procedure is created.
- The stored procedure name follows the naming convention.
- The standard script header is included.
- The merge logic updates rows only when a value changes.
- Full-load delete logic is included when applicable.
- Incremental-load delete logic is excluded when applicable.
- The script is run and the merge of data from the land table to the source table is successful.
- The stored procedure is attached or linked in the PBI.

---

## 06. Elevate DDL scripts to prod

### Description
Elevate the land table DDL and source table DDL to prod using the pipeline process.

### Acceptance Criteria
- The land table DDL is pipeline elevated.
- The source table DDL is pipeline elevated.
- Prod contains the expected table structures.
- Any elevation issues are documented in the PBI.

---

## 07. Load source table in prod

### Description
Run the initial extract script and load the source table in prod. Load the full extract or create an incremental load script if the table volume is over 10 million records. Review the load results and document any issues found.

### Acceptance Criteria
- The initial source load is executed.
- The load completes without unresolved errors.
- The source table contains expected data after the load.
- Any issues found during the initial source load are documented in the PBI.

---

## 08. Elevate the land-to-source merge SP

### Description
Elevate the land-to-source stored procedure to prod using the pipeline process.

### Acceptance Criteria
- The stored procedure is elevated.
- The stored procedure is available in prod.
- Any elevation issues are documented in the PBI.

---

## 09. Test merge SP in prod

### Description
Run the land-to-source stored procedure in production. Review the results and confirm that inserts, updates, and deletes behave correctly for the identified load type.

### Acceptance Criteria
- The stored procedure is executed in production.
- The production execution completes without unresolved errors.
- Insert behavior is confirmed.
- Update behavior is confirmed.
- Delete behavior is confirmed when applicable.
- Any production test issues are documented in the PBI.

---

## 10. Configure job control

### Description
Configure job control for the new table load based on the production test results. Add or update the job control settings required to run the table on the selected load pattern.

### Acceptance Criteria
- Job control is configured for the table load.
- The configuration matches the identified load type.
- The configuration reflects the production-tested process.
- Configuration details are documented in the PBI.

---

## 11. Add SSIS source container

### Description
Add the source table container to the SSIS solution after job control is configured. Build the container steps based on the identified load type. For full loads, include the full-load process steps. For incremental loads, include the incremental-load process steps.

### Acceptance Criteria
- A source table container is added to the SSIS solution.
- The container name follows the naming convention.
- The container matches the identified load type.
- The SSIS changes are attached or linked in the PBI.

---

## 12. Configure weekly reconciliation

### Description
Configure weekly reconciliation in Environmental Health for the new source table.

### Acceptance Criteria
- Weekly reconciliation is configured in Environmental Health.
- The reconciliation is tied to the new source table.
- Configuration details are documented in the PBI.

---

## 13. Elevate SSIS package to prod

### Description
Elevate the SSIS solution changes to prod using the pipeline process.

### Acceptance Criteria
- The SSIS solution changes are pipeline elevated.
- The updated package is available in prod.
- Any elevation issues are documented in the PBI.

---

## 14. Obtain warehouse SME signoff

### Description
Schedule the final review with the warehouse SME. Review the completed table-ingestion work, including the DDL, stored procedure, production test results, job control configuration, SSIS changes, and weekly reconciliation. Record the warehouse SME decision in the PBI.

### Acceptance Criteria
- The warehouse SME review is completed.
- The warehouse SME approval is documented in the PBI.
- Any issues raised during the review are documented and resolved.
- The PBI is ready to close.