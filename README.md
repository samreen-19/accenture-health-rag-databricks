# Intelligent Document Parsing (IDP) Agent for Regional Healthcare in Ghana

**A Databricks-powered Agentic AI intelligence layer that extracts medical facility capabilities from messy, unstructured data, reasons over it to detect suspicious claims, and maps critical medical deserts.** **[Watch the 5-Minute Project Demo Here](YOUR_YOUTUBE_OR_DRIVE_LINK)**

## The Challenge & Motivation
By 2030, the world will face a massive shortage of healthcare workers—a planetary-scale coordination failure. This project targets the hackathon's ambitious goal: to drastically reduce the time it takes for patients to receive lifesaving treatment by using AI to bridge the gap between medical expertise and the hospitals that urgently need them. 

## Goals Achieved

**1. Unstructured Feature Extraction**
* Processes free-form text fields from the Virtue Foundation Ghana dataset (procedures, equipment, and capabilities).
* Uses Databricks Serverless AI Gateway (`meta-llama-3-3-70b-instruct`) to strictly map unstructured insights into the structured schema (Pydantic).

**2. Anomaly Detection**
* Built a smart **planning and reasoning system** that automatically cross-references extracted clinical claims against structural facility capacities.
* Successfully detects incomplete or suspicious claims (e.g., flagging a clinic claiming to perform advanced surgeries while reporting zero doctors).

**3. Medical Desert Mapping**
* Combines intelligent synthesis with an interactive Plotly Express geospatial dashboard.
* Visually identifies "medical deserts," mapping where critical expertise is missing and where patients are at risk, aiding NGOs in resource allocation.

**4. User Experience**
* **Interactive RAG Assistant:** An AI agent powered by Databricks Serverless Vector Search that retrieves audited clinical facts to accurately answer NGO planners' natural language questions.


## Architecture & Tech Stack
Built entirely on the **Databricks Data Intelligence Platform**
* **Serverless Notebook Compute:** Instant, on-demand processing power to run the pandas/Spark data engineering pipeline.
* **Databricks AI Gateway:** `meta-llama-3-3-70b-instruct` endpoint for structured IDP extraction.
* **Serverless Vector Search:** `databricks-qwen3-embedding-0-6b` for semantic retrieval and auto-syncing indexing.
* **Unity Catalog & Delta Lake:** Managed tables with **Change Data Feed (CDF)** enabled for auto-syncing data updates.

## Instructions to Run on Databricks

**Workspace & Compute Requirements:**
* A Databricks workspace with Unity Catalog enabled.
* **Serverless Compute** enabled for notebook execution 
* Access to Serverless Foundation Model Serving, specifically the `databricks-meta-llama-3-3-70b-instruct` endpoint.

**Data Setup:**
1. Download the raw CSV dataset: [Virtue Foundation Ghana v0.3 - Sheet1.csv](https://drive.google.com/file/d/1qgmLHrJYu8TKY2UeQ-VFD4PQ_avPoZ3d/view).
2. Upload the CSV into a Unity Catalog Volume. 
3. Open the imported `accenture_hackathon_final.ipynb` notebook and navigate to **Section 7**. Update the `file_path` variable to point to the exact volume path where you uploaded the CSV.

**Execution Steps:**
1. Attach the notebook to **Serverless** compute (or your standard cluster) and click **Run All** at the top of the notebook.
2. **Section 1** will automatically install the necessary pipeline dependencies (`langgraph`, `mlflow`, `pydantic`, `databricks-sdk`). *Note: You may need to restart the Python kernel after this first cell runs.*
3. The pipeline handles authentication automatically using the notebook's context token to hit the AI Gateway. 
4. The ingestion loop in **Section 9** includes a 1.5-second `time.sleep()` delay to respect endpoint rate limits. Please allow a few minutes for the batch to process completely.
5. The final audited output will automatically be registered as a managed Delta Table in Unity Catalog at `workspace.default.audited_healthcare_facilities_final` with Change Data Feed (CDF) enabled.
