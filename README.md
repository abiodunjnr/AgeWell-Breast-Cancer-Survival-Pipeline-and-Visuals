
# 🩺 AgeWell Breast Cancer Survival Pipeline — Azure ADF + ADLS + Power BI


A production-style data end-to-end pipeline built on Azure to turn a 2024 breast-cancer research dataset into trusted, refreshable insights.
It covers ingestion, transformation, orchestration, storage design, and dashboarding—ready for portfolio and interview conversations.


> **Note:** This is an educational analytics project using research data. No clinical claims are made.


---


## 📌 Project Summary


* **Goal:** Deliver clear survival insights (alive vs. dead, age bands, tumor stage/size, receptor status) for stakeholders.
* **Flow:** GitHub (CSV) → Azure Data Factory → ADLS Gen2 (Medallion) → Power BI.
* **Outputs:** Curated datasets in the lake and an interactive Power BI report with KPIs and quick insights.
* **Automation:** Scheduled ADF triggers with monitored runs for reliability.


---


## 🧱 Architecture Overview


* **Source:** CSV files hosted on GitHub (HTTP).
* **Orchestration:** Azure Data Factory (Copy activity + Mapping Data Flows running Spark).
* **Storage:** ADLS Gen2 with Medallion design


  * **Silver:** standardized, typed data
  * **Gold:** presentation data for BI (Parquet recommended; CSV supported in this demo)
* **Serving:** Power BI connected to the Gold layer, published to Power BI Service for collaboration.


*(Architecture and pipeline screenshots enclosed within this repo.)*


---


## 🔍 Business Context


The project simulates a hospital research workflow: prepare research data consistently, separate alive vs. deceased cohorts, and surface survival trends by age, stage, tumor size, race, and hormone receptor status. The dashboard focuses on clarity, not clinical diagnosis.


---


## 🧠 Key Features & Highlights


**Data Engineering**


* Parameterized ADF pipeline to copy from GitHub over HTTP.
* Mapping Data Flows (Spark) for schema projection, type casting, renaming, and column pruning.
* Removed dataset fields not useful for analysis in this context (e.g., *6th Stage* as redundant, *A-Stage* ambiguous).
* **Conditional Split**: routes records by `Status` into **clinic (alive)** and **morgue (dead)** paths.
* Gold outputs organized for BI consumption (partition-friendly layout).


**Analytics & Reporting**


* Power BI model with clear KPIs and slicers.
* **Quick Insights** mini-table highlighting top age bands by mortality and median survival.
* **Combo visual** showing stage vs. outcomes (average tumor size with survival rate line).
* Patient table with status indicators for row-level context.


**Ops & Governance**


* Scheduled triggers; run history visible in ADF Monitor.
* Publish/validate workflow for pipeline changes.
* Ready for RBAC and Managed Identity (recommended for production).


---


## 🗂️ Storage Design (Medallion)


* **Silver:** cleaned and typed data (standardized column names and categories).
* **Gold:** curated presentation data (alive vs. dead directories; load-date folders).
* **Why this matters:** predictable paths, easier partitioning, and faster BI refreshes—especially with Parquet.


---


## 🔄 Pipeline Steps (what happens end-to-end)


1. **Create** resource group and ADLS containers (silver, gold) following Medallion conventions.
2. **Connect** ADF to Storage (linked service) and to GitHub (HTTP).
3. **Pipeline 1 – Copy:** pull CSV from GitHub using parameterized base/relative URLs into the lake.
4. **Pipeline 2 – Data Flow:**


   * Enable debug, import projection to infer schema.
   * Clean/standardize; remove redundant & ambiguous fields.
   * **Split** into ward/clinic (alive) and morgue (dead).
   * **Sink** to Gold with organized directories (clinic and morgue).
5. **Schedule & Monitor:** set triggers; review Pipeline/Trigger runs.
6. **Visualize:** connect Power BI to Gold and build the report; publish to the Service.


---


## 📊 Power BI Dashboard (what’s included)


**KPIs**


* Total Patients • Alive Patients • Dead Patients
* Median Survival (months) • Average Tumor Size (units noted in the report)


**Core Visuals**


* **Alive vs. Dead** donut (counts + percentages)
* **Quick Insights — Age Groups** table (Mortality %, Patients, Median Survival)
* **Stage vs. Survival** combo chart (Avg Tumor Size + Survival Rate line)
* **Patient** table with status indicator


**Slicers**


* Age • Tumor Stage • Race • Estrogen Status • Tumor Grade


**Publishing**


* Report published to Power BI Service for collaboration and scheduled refresh.


---


## 🛠️ Technology Stack


* **Programming & Compute:** Spark (via ADF Mapping Data Flows)
* **Storage:** Azure Data Lake Storage Gen2 (Silver/Gold; Parquet recommended)
* **Orchestration:** Azure Data Factory (pipelines, parameters, triggers, Monitor)
* **BI & Reporting:** Power BI Desktop & Power BI Service
* **Source Control:** GitHub (raw data hosting and repo)


---


## 🚀 Getting Started (replication guide)


* Provision Azure resources (Resource Group, ADLS Gen2, ADF, Power BI workspace).
* Add ADF **linked services** for HTTP (GitHub) and Storage.
* Import the **Copy** and **Data Flow** pipelines; configure parameters for URLs and lake paths.
* Create schedule **triggers** and validate/publish.
* Connect Power BI to the **Gold** container folders and build the report.
* Publish the PBIX to the Service and configure refresh.



---


## ✅ Outcomes (example insights)

* Majority of patients in this research snapshot are alive at follow-up.
* Higher mortality appears in older age bands (e.g., 60–69, 80+).
* Tumor size/stage trends align with expected survival patterns.
* Cohort splits (clinic vs. morgue) enable focused analysis.


*Insights are dataset-specific and for demonstration only.*


---


## 📂 Repository Layout


* `adf/` — exported ADF objects (pipelines, data flows, datasets)
* `powerbi/` — PBIX and theme assets
* `docs/` — screenshots (architecture, pipelines, runs, storage, dashboard)
* `storage/` — path conventions and examples
* `README.md` — this overview


---


## 🧭 Next Steps / Enhancements


* Land raw files in **Bronze** (immutable) and promote Parquet in Gold for performance.
* Add basic **data quality** checks and a quarantine folder; show a “quarantined rows” tile in the report.
* Azure Monitor **alerts** to email/Teams on failures.
* Metadata-driven ingestion (control table) to scale to new sources.
* Lineage snapshot with Microsoft Purview.
* CI/CD (ARM/Bicep + GitHub Actions) for environment promotion.


---


## 🔐 Ethics & Privacy


* Uses a research dataset for educational analytics.
* No PII/PHI is shared in the repository or screenshots.
* The dashboard and findings are **not** medical advice.


---


## 👤 About the Author


**Name:** Toluwani Adefisoye
**Email:** [toluenenate@gmail.com](mailto:toluenenate@gmail.com)
**GitHub:** github.com/abiodunjnr
**LinkedIn:** [https://www.linkedin.com/in/toluwani-adefisoye-766ab5221/](https://www.linkedin.com/in/toluwani-adefisoye-766ab5221/)


---


