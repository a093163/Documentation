# **The Claims Manager Metric Report (CMMR) Journey**

Welcome to the **Claims Manager Metric Report (CMMR)** journey! This document is designed to onboard you to the history, goals, challenges, and architecture of the CMMR project. It will help you understand not just the technical aspects, but also the reasoning behind the decisions made during the development of the metric data warehouse.

This documentation is modular and optimized for use in a **Git repository**, ensuring easy navigation and AI-friendly searchability.

---

## **Table of Contents**
1. [Overview](#overview)
2. [Requirements Gathering](#requirements-gathering)
3. [Data Modeling](#data-modeling)
4. [Data Integration (ETL/ELT Processes)](#data-integration-etl-elt-processes)
5. [Data Storage and Optimization](#data-storage-and-optimization)
6. [Data Governance and Security](#data-governance-and-security)
7. [Performance Tuning](#performance-tuning)
8. [Reporting and Analytics Integration](#reporting-and-analytics-integration)
9. [Maintenance and Monitoring](#maintenance-and-monitoring)

---

## **Overview**
### **Purpose**
The CMMR project was initiated to consolidate, simplify, and modernize claims reporting infrastructure. It addresses challenges like duplication, inefficiency, and scalability, while enabling visually guided decision-making.

### **Tags:** 
`CMMR`, `Metric Data Warehouse`, `Architecture`, `ETL`, `Governance`, `Optimization`, `Reporting Integration`

---

## **Requirements Gathering**
### **Task:** 
Understand the business needs, reporting requirements, and the types of data to be stored and analyzed.

### **Background**
Claims Control has been tasked with reporting key metrics for over a decade. Early reports, such as the **Senior Leadership Reporting Package (SLRP)** and its spinoff, the **Central Services Organization SLRP (CSO SLRP)**, laid the foundation for leadership reporting. Over time, additional reports like **OPMP**, **WCGW (What Could Go Wrong)**, and the **One Pager** followed, collectively reporting on around **300 metrics**. However:
- Only **half of these metrics** were unique, resulting in duplication and inefficiencies.
- By 2018, Claims Control launched the **Claims Key Metrics Project** to address these challenges.

### **Goals of the Claims Key Metrics Project:**
1. Consolidate and simplify reporting to reduce duplication and inefficiency.
2. Increase speed-to-market for new metrics and reporting changes.
3. Build a nimble infrastructure to support evolving business needs.
4. Enable visually guided decision-making through improved reporting tools.

These goals required technical expertise to design an architecture that supported **dynamic and scalable reporting**.

---

### **Commercial Lines (CL) Manager Report Integration**
In 2021, a separate project was started to address the needs of the evolving **Commercial Lines (CL)** organization, ultimately creating the **CL Manager Report**. This project began with a simple request to develop a new scorecard containing specific metrics related to CL file owners and estimators. Over several years, the report evolved significantly:
- By 2023, the report contained **65 different metrics** and **seven different tabs**.
- The cost of maintaining the report outweighed its benefits due to the following issues:
  - **Data accuracy problems.**
  - A need for **more metrics**.
  - An **unclear and inadequate architecture**.
  - An **inability to integrate into batch processing**.
- Additionally, the customer requested converting the report from **Tableau** to **Power BI**.

In 2023, a new project was spun up to rewrite the **CL Manager Report** and address these issues. The initial phases involved:
1. **Researching the report's structure and metric definitions.**
2. Recognizing the similarities between the CL Manager Report and the **Key Metrics Report**.

After additional research, it was determined that **migrating the CL Manager metrics to the metric data warehouse architecture** was the most cost-effective and time-efficient solution to address the identified issues. This migration allowed for:
- Resolving architectural and quality issues.
- Adding the necessary components to publish a new report, which became known as the **Claims Manager Metric Report (CMMR)**.

---

### **Key Activities**
#### **1. Identifying Stakeholders and Their Needs**
- **Stakeholders:**
  - **Consumers:** Claims and process senior leadership.
  - **Report Owners:** Claims Control analysts.
  - **Metric Owners:** SMEs from Claims Control and external areas (e.g., PBL and Policy teams).
- **Challenges:**
  - Analysts wanted prioritization, but SMEs were often busy with other responsibilities.
  - Leadership cared about outcomes but didn’t engage in the details.
  - Coordination across these groups was difficult due to competing priorities.

#### **2. Defining Key Metrics and Reporting Requirements**
- **Patterns in Metrics:**
  - Metrics exist at specific grains (levels of detail).
  - Common attributes (e.g., global filters) were identified across metrics.
  - Each metric has unique elements, such as definitions for **date anchors**, **geo anchors**, and **rep anchors**.
- **Outcome:** These patterns helped design a flexible system to support both shared and unique metric elements.

#### **3. Understanding Data Sources and Their Structure**
- **Data Sources:**
  - SQL, DB2, Oracle, Prostar, BISS, and other platforms.
- **Challenges:**
  - Blending data from diverse sources was technically complex.
  - Many metrics required combining data from multiple systems.

---

## **Data Modeling**
### **Task:** 
Design the logical and physical structure of the data warehouse to support dynamic reporting and scalability.

### **Key Activities**
#### **1. Conceptual Model**
- Focused on high-level data entities and relationships between metrics.

#### **2. Logical Model**
- Prototyped the **Metric Stack** (later called the **Metric Mart**).
- **Why the Metric Mart?**
  - Allowed unique metric definitions while using a single filter for exploration.
  - Narrow table design optimized performance for large data volumes.
  - Included global filters, reducing the need for multiple tables.
  - Minimized DAX calculations by stacking metrics dynamically.

#### **3. Physical Model**
- **Decisions:**
  - Replicated external tables locally in the warehouse.
  - Added fields like `rowversion`, `create date`, `modify date`, and `soft delete date` for incremental loading.
  - Developed job control processes for weekly incremental checkpoints.
  - Introduced **temporal tables** mid-project to recreate fact values at specific points in time.

---

### **Lessons Learned**
- Use templated approaches for common tasks (e.g., merge processes).
- Format scripts with proper indentation for readability and debugging.
- Adopt standard naming conventions for tables and keys.
- Rare customizations are acceptable for recreating existing star schema data marts.

---

## **Data Integration (ETL/ELT Processes)**
### **Task:** 
Extract, transform, and load data from various sources into the warehouse.

### **Key Activities**
1. **Connect to Data Sources:**
   - Databases, APIs, and flat files.
2. **Clean and Transform Data:**
   - Ensure consistency and compatibility across sources.
3. **Load Data into Warehouse:**
   - Use staging areas, fact tables, and dimension tables.
4. **Tools:**
   - Use **SSIS** for weekly refreshes.
   - Prioritize **stored procedures** for performance tuning in SQL.

---

## **Data Storage and Optimization**
### **Task:** 
Design how data will be stored and accessed efficiently.

### **Key Activities**
1. Use **columnstore indexes** for large fact tables to optimize query performance.
2. Implement **incremental loads** for large tables to avoid overloading transaction logs.
3. Optimize storage for scalability and growth.

---

## **Data Governance and Security**
### **Task:** 
Ensure data quality, compliance, and security.

### **Key Activities**
1. Establish validation rules for data accuracy.
2. Define access controls to manage permissions.
3. Ensure compliance with regulations (e.g., GDPR, HIPAA).
4. Implement monitoring and auditing mechanisms.

---

## **Performance Tuning**
### **Task:** 
Optimize the warehouse for faster query execution and efficient resource usage.

### **Key Activities**
1. Analyze query patterns to optimize indexes, partitions, and caching.
2. Implement **materialized views** for frequently accessed data.
3. Continuously monitor system performance and adjust configurations.

---

## **Reporting and Analytics Integration**
### **Task:** 
Enable reporting and analytics tools to connect to the warehouse.

### **Key Activities**
1. Design views and interfaces for BI tools (e.g., Tableau, Power BI, Looker).
2. Provide data marts or aggregated tables tailored to business needs.

---

## **Maintenance and Monitoring**
### **Task:** 
Continuously monitor and maintain the warehouse to ensure reliability.

### **Key Activities**
1. Monitor data pipelines and ETL processes for failures.
2. Perform regular backups and disaster recovery planning.
3. Address data drift or schema changes in source systems.

---

## **Conclusion**
This onboarding document provides the context and reasoning behind the development of the CMMR data warehouse. By understanding the **what** and **why**, you’ll gain insight into the challenges, solutions, and lessons learned that shaped the architecture.

Welcome aboard the **CMMR journey**!

---

### **AI Optimization Notes**
- **Searchable Tags:** Use keywords like `Metric Mart`, `ETL`, `Data Modeling`, `Governance`, `Optimization`, `Reporting Integration`.
- **Modular Structure:** Each section is independent and can be queried directly.
- **Internal Links:** Use anchor tags for easy navigation within the document.
- **Examples and Context:** Provide real-world scenarios for clarity.

This structure ensures the document is **AI-friendly** for quick retrieval and easy navigation within a Git repository. Let me know if you'd like further refinements!
