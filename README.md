# 🏥 Agentic RAG Pipeline for Regional Healthcare Resource Validation

**A Databricks-powered Agentic RAG assistant that cleans regional healthcare logs, runs automated rule-based reasoning to flag medical anomalies, and answers user questions.** 

🎥 **[Watch the 5-Minute Project Demo Here](YOUR_YOUTUBE_OR_DRIVE_LINK)**

## 🚀 The Challenge
Healthcare resource datasets are often messy, unstructured, and filled with conflicting information. We needed a way to automatically ingest raw CSV data, structure it, and algorithmically audit it for critical medical anomalies (e.g., a clinic claiming to perform advanced surgeries while reporting zero doctors on staff).

## 🛠️ Architecture & Tech Stack
This project is built entirely on the **Databricks Data Intelligence Platform**, heavily leveraging **Serverless Compute** for instant scalability:
* **Serverless Notebook Compute:** Instant, on-demand processing power to run the pandas/Spark data engineering pipeline without managing cluster infrastructure.
* **Serverless AI Gateway:** `meta-llama-3-3-70b-instruct` endpoint for strictly formatted Pydantic JSON extraction.
* **Serverless Vector Search:** `databricks-qwen3-embedding-0-6b` for semantic retrieval 
* **Unity Catalog & Delta Lake:** Managed tables with **Change Data Feed (CDF)** enabled for auto-syncing data updates.
* **MLflow Tracing:** Deep execution monitoring to track multi-agent extraction loops and latency.
* **Plotly Express:** A geospatial dashboard mapping compliant vs. flagged facilities.

## ✨ Key Features
1. **Safe Ingestion Loop:** Processes raw text fields into a unified context block, executing a sequential LLM loop with a 1.5-second sleep delay to protect API limits.
2. **Smart Anomaly Checking:** A Python deterministic reasoning engine that cross-references extracted clinical claims against structural facility capacities.
3. **Live Pipeline Tracking:** MLflow spans (`mlflow.start_span`) track precisely how data moves through extraction and auditing steps in real-time.
4. **Auto-Syncing Search Index:** Delta table CDF automatically pushes any data modifications to our Serverless Vector Search index without manual pipelines.
5. **Interactive RAG Assistant:** An AI agent powered by Databricks Vector Search that instantly retrieves audited clinical facts to accurately answer user questions about regional healthcare resources in Ghana

## 💡 Business Value & Impact
* **Rapid Anomaly Detection:** Reduces manual auditing hours by automatically flagging high-risk operational discrepancies.
* **Resource Optimization:** Helps regional health planners identify areas lacking critical infrastructure (e.g., regions performing surgeries without backup power).
* **Scalable Governance:** The automated Delta Lake CDF sync means new data can be streamed, audited, and indexed continuously without rebuilding the pipeline.

## 📊 Pipeline Visuals
**Geospatial Compliance Dashboard:**
*(Upload your map screenshot to the repo and add the filename here: `![Dashboard Map](filename.png)`)*

**MLflow Execution Trace:**
*(Upload your trace screenshot to the repo and add the filename here: `![MLflow Trace](filename.png)`)*

## ⚙️ Instructions to Run on Databricks

**Workspace & Compute Requirements:**
* A Databricks workspace with Unity Catalog enabled.
* **Serverless Compute** enabled for notebook execution (or a standard Databricks Runtime 13.3 LTS ML cluster).
* Access to Serverless Foundation Model Serving, specifically the `databricks-meta-llama-3-3-70b-instruct` endpoint.

**Data Setup:**
1. Download the raw CSV dataset (`Virtue Foundation Ghana v0.3 - Sheet1.csv`).
2. Upload the CSV into a Unity Catalog Volume. 
3. Open the imported `accenture_hackathon_final.ipynb` notebook and navigate to **Section 7**. Update the `file_path` variable to point to the exact volume path where you uploaded the CSV.

**Execution Steps:**
1. Attach the notebook to **Serverless** compute and click **Run All** at the top of the notebook.
2. **Section 1** will automatically install the necessary pipeline dependencies (`langgraph`, `mlflow`, `pydantic`, `databricks-sdk`). 
3. The pipeline handles authentication automatically using the notebook's context token to hit the AI Gateway. 
4. The ingestion loop in **Section 9** includes a 1.5-second `time.sleep()` delay to respect endpoint rate limits. 
5. The final audited output will automatically be registered as a managed Delta Table in Unity Catalog at `workspace.default.audited_healthcare_facilities_final` with Change Data Feed (CDF) enabled.
