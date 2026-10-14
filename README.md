# Allocation Planner

A simple app for planners: review the monthly allocation plan, make the changes from the planner input, and publish it so reports can use it.

**Login:** planner only. Demo account: `yogesh` / `planner123` (turned off once real users are set in Secrets).

| Page | What the planner does |
|---|---|
| **Step 1 · Review & adjust** | Check the month's plan and answer the four planner-input questions: storage types to include, min / max penetration per building, building capacity (incl. shared capacity like Bailey Road FLA + HFG), departments to exclude. The plan and checks update straight away. |
| **Step 2 · Publish** | See what will be published, choose to replace or keep an earlier version for the month, publish, download as Excel, and see the publishing history. |

Technical settings (project, dataset, location) sit in a closed **Destination settings (for admins)** section.
Every change the planner makes is saved with the plan in `run_log.adjustments_json`.
Tables written (partitioned by month on `target_month`): `allocation_plan`, `capacity_summary`, `warehouse_share`, `run_log`.

## Run locally
```bash
pip install -r requirements.txt
streamlit run app.py
```

## Deploy on Streamlit Community Cloud
1. Push this repo to GitHub.
2. share.streamlit.io → **Create app** → pick the repo, branch `main`, main file `app.py`.
3. (Optional) **Advanced settings → Secrets**: add users and the BigQuery key (see `.streamlit/secrets.toml.example`).
4. Deploy and share the link.

## Users
```bash
python app.py adduser <username> "<Full Name>" <password>
```
Paste the printed block into Secrets.

## Real BigQuery (connected mode)
Without a key the app runs in **demo mode** (a local SQLite file stands in for BigQuery).
To write to real BigQuery: add `[gcp_service_account]` and `[bigquery]` to Secrets and uncomment the
Google lines in `requirements.txt`. The service account needs BigQuery Data Editor + Job User on the
dataset (and Storage Object Viewer to read `results_output.pkl` from GCS).

## Notes
- Only upload `.pkl` files produced by your own pipeline — pickle files can run code when opened.
- Publish history is stored in `data/` on the app server and is reset when Streamlit Cloud restarts the app.
