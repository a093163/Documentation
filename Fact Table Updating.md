# PBI

## Title
CMMR: Feature – Fact Table – Update fact table for ingestion of [Metric Name]

## Description
Identify required fields from the source tables referenced in the validated metric definition and confirm whether existing fields or overlapping logic can be reused. Determine whether each field should be implemented as a reusable fact column or through a dimension reference based on attribute type and alignment to the fact grain. Avoid metric-specific indicator fields when a reusable underlying fact value can support current and future reporting needs. Integrate approved fields into the fact table DDL, initial load script, and weekly merge script. Elevate the required database objects and scripts to production. Execute the initial fact table load after production elevation, confirm successful completion, and manually execute the weekly merge script.

## Assumptions
- The respective metric(s) have been validated successfully against the internal definition.
- All required dimension table references already exist.

## Acceptance Criteria
- The existing script is reviewed for any relevant logic.
- The field names have been approved by the DW team.
- The new fact(s) are incorporated into the existing initial load script.
- The new fact(s) are added to the existing DDL.
- The fact table is loaded with sample data.
- The new fact(s) are added to the existing merge script.
- The DDL script is elevated.
- The initial load script is executed.
- The merge script is elevated.

# Tasks

## 01. Review existing script for relevant logic

### Description
Review the existing script for any relevant logic.

### Acceptance Criteria
- The existing script is reviewed for any relevant logic.

---

## 02. Create new field names (DW approval)

### Description
Create new field names (DW approval).

### Acceptance Criteria
- Create new field names (DW approval).

---

## 03. Add new fields(s) to initial load script

### Description
Add new fields(s) to initial load script.

### Acceptance Criteria
- Add new fields(s) to initial load script.

---

## 04. Add new fields(s) to existing DDL

### Description
Add new fields(s) to existing DDL.

### Acceptance Criteria
- Add new fields(s) to existing DDL.

---

## 05. Load fact table with sample data

### Description
Load the fact table with sample data.

### Acceptance Criteria
- The fact table is loaded with sample data.

---

## 06. Add new fact(s) to existing merge script

### Description
Add the new fact(s) to the existing merge script.

### Acceptance Criteria
- The new fact(s) are added to the existing merge script.

---

## 07. Elevate DDL script

### Description
Elevate the DDL script.

### Acceptance Criteria
- The DDL script is elevated.

---

## 08. Execute initial load script

### Description
Execute the initial load script.

### Acceptance Criteria
- The initial load script is executed.

---

## 09. Elevate merge script

### Description
Elevate the merge script.

### Acceptance Criteria
- The merge script is elevated.

---

## 10. Execute weekly merge script

### Description
Execute the weekly merge script.

### Acceptance Criteria
- The weekly merge script is elevated.