# **CMMR Dashboard Metric Addition Workflow**

This documentation outlines the workflow for adding a new metric to the **CMMR Dashboard**. It is organized into modular sections to ensure that AI tools can quickly locate relevant instructions based on user queries. Each section is tagged with keywords and structured for efficient parsing.

---

## **Table of Contents**
1. [Overview](#overview)
2. [Requirement Gathering](#requirement-gathering)
3. [Metric Definition](#metric-definition)
4. [Metric Ingestion](#metric-ingestion)
5. [Common Scenarios and Examples](#common-scenarios-and-examples)
6. [FAQ](#faq)

---

## **Overview**
### **Purpose**
This workflow ensures the efficient addition of metrics to the CMMR dashboard, including:
- **Requirement Gathering:** Collecting necessary information from the requestor or SME.
- **Metric Definition:** Creating an accurate metric definition and obtaining signoff.
- **Metric Ingestion:** Validating, reconciling, and ingesting the metric into the data warehouse.

### **Tags:** 
`CMMR`, `Metric Addition`, `Workflow`, `Requirement Gathering`, `Metric Definition`, `Metric Ingestion`

---

## **Requirement Gathering**
### **Purpose**
Collect all necessary information to define the metric. This step ensures that the metric is well-documented and ready for definition.

### **Steps**
1. **Understand the Metric:**
   - **Key Questions:** 
     - What is the metric, and why is it being requested?
     - What is the **eligible population** (e.g., WHERE clause)?
     - What is the **actual population** (subset of eligible population)?
     - Can the population be counted across multiple months or only in a specific month?
   - **Examples:**
     - Eligible population: All claim properties.
     - Actual population: Only properties that are total losses.

2. **Gather Detailed Requirements:**
   - Ideal sources:
     - **DG Certified Logic**
     - **Production Reports**
   - If unavailable, requestor must involve an SME to define the metric.

3. **Confirm Sponsorship:**
   - Identify the **requestor** and their **manager** (sponsor).

4. **Sensitive Data Classification:**
   - Determine if the metric is sensitive or highly sensitive.

5. **Metric Naming Standards:**
   - Provide a name following **naming standards** (aliases for older names can be included).

6. **Anchors and Format:**
   - Identify:
     - **Date anchor**
     - **Geo anchor**
     - **Rep anchor**
   - Define preferred result format and whether the metric result is **inverse** (up = bad, down = good).

---

## **Metric Definition**
### **Purpose**
Create a concise and efficient metric definition that is ready for ingestion and customer signoff.

### **Steps**
1. **Understand Provided Logic:**
   - Work backwards from the end to the beginning:
     - Numerator, denominator (if applicable).
     - Date, geo, and rep anchors.
     - Filtering conditions (e.g., WHERE clauses, inline join filters, inner joins).
     - Identify manual values requiring updates and unstructured data sources (e.g., Excel files).

2. **Refactor Code:**
   - Simplify logic to remove unnecessary processing.
   - Use **single-step code** where possible.
   - If multiple sources are required:
     - Use **SAS** to extract and merge data.
     - (Future: Python may be used if stakeholders can validate scripts.)

3. **Add Metric to DimMetric Table:**
   - Document:
     - Date, geo, and rep anchors.
     - Grain (e.g., monthly, daily).
     - Fact table where the metric resides.
     - Calculation type (limited to dashboard-defined types).
     - Metric lag (development time before publishing results).

4. **Confirm Table Availability:**
   - Ensure all identified tables are in the metric data warehouse.
   - Verify fact fields in the fact table.

5. **Formalize Documentation:**
   - Create detailed documentation for the metric definition.
   - Include refactored logic and supporting details.
   - Send documentation to the requestor for **signoff**.

6. **Git Repo Updates:**
   - Move "Pending Signoff / Feedback" task to **In Progress**.
   - Update the PBI card to "Waiting on Customer QA Signoff."

---

## **Metric Ingestion**
### **Purpose**
Ingest the metric into the data warehouse, reconcile results, and enable the metric for consumption in the dashboard.

### **Steps**
1. **Confirm External Table Availability:**
   - Verify all external tables (or substitutes) are in the metric data warehouse.
   - Prioritize ingestion for missing tables.

2. **Refactor Metric Definition:**
   - Use only source tables (exceptions: DimOrg, DimMonthlyOrg, DimDate).
   - Reconcile metric definition with internal definition by business keys.
   - Document any rare exceptions.

3. **Create Basic Metric Extract:**
   - Ignore global filters initially.
   - Use a single fact table and dimension tables.
   - Add missing fields to the fact table if necessary.

4. **Add Global Filters:**
   - Apply global filters to the extract.
   - Process using the **standard stored procedure (SP)** for metric ingestion.

5. **Reconcile Results:**
   - Reconcile metric extract script with metric mart results.
   - Reconcile aggregated results with the dashboard using at least one organization filter.

6. **QA Signoff:**
   - Obtain signoff from the project SME.
   - QA signoff requires:
     - Metric definition documentation.
     - Customer’s signoff.
     - Clean or well-documented reconciliation results.
     - All customer discussions documented.

7. **Activate Metric:**
   - Set the metric to **active** in the dashboard.

---

## **Common Scenarios and Examples**
### **Scenario 1: Eligible vs. Actual Population**
- Eligible Population: All claims.
- Actual Population: Claims that are total losses.

### **Scenario 2: Multi-Month Tracking**
- A claim can be open across multiple months (inventory metric).
- A claim can only be closed in a single month.

### **Scenario 3: Missing Fact Table Fields**
- If a required field is missing, add it to the fact table before ingestion.

---

## **FAQ**
### **Q1: What happens if requirements are incomplete?**
- Create a spike card to gather missing requirements. Collaborate with the requestor and SME to fill gaps.

### **Q2: How do we handle sensitive metrics?**
- Follow company standards for sensitive or highly sensitive data classification.

### **Q3: Can Python be used for metric extraction?**
- Python may be used in the future, but ensure all stakeholders can validate Python scripts.

### **Q4: What if a required table is missing from the data warehouse?**
- Prioritize ingestion of the missing table before proceeding with metric ingestion.

---

## **AI Optimization Notes**
- **Searchable Tags:** Use keywords like `CMMR`, `Metric Definition`, `Metric Ingestion`, `Requirement Gathering`, `Sensitive Metrics`, `Fact Table`.
- **Modular Structure:** Each section is independent and can be queried directly.
- **Clear Headings:** AI can parse headings to locate relevant content quickly.
- **Examples and Scenarios:** Provide context for common queries.
- **FAQ Section:** Anticipate frequently asked questions and provide concise answers.

---

This structure is designed to ensure AI tools can parse, locate, and retrieve instructions efficiently when users ask general or specific questions in a Git repository. Let me know if you'd like further refinements!
