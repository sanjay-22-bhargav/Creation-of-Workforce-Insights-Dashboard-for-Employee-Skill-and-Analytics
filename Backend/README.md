# Backend

Flask API backend for the **Workforce Insights Dashboard**, providing employee analytics, dashboard data, prediction endpoints, and chat functionality.

## Features

- Dashboard and workforce analytics endpoints
- Employee data access
- Attrition and promotion prediction services
- Chat endpoint for workforce insights
- CSV and Excel dataset loading with column cleanup and duplicate removal
- CORS enabled for frontend integration

## Project structure

- `app.py` — Creates the Flask application, loads the dataset, and registers API blueprints.
- `data_loader.py` — Loads CSV/XLS/XLSX datasets, normalizes column names, removes duplicates, and converts records to JSON-friendly dictionaries.
- `dashboard_routes.py` — Dashboard endpoints.
- `employee_routes.py` — Employee endpoints.
- `analytics_routes.py` — Workforce analytics endpoints.
- `prediction_routes.py` — Prediction API endpoints.
- `prediction_service.py` — Prediction logic.
- `attrition_model.py` — Attrition model functionality.
- `promotion_model.py` — Promotion model functionality.
- `chat_routes.py` and `chat_service.py` — Workforce analytics chat functionality.
- `requirements.txt` — Python dependencies.

## Setup

From the repository root, create and activate a virtual environment:

```bash
python -m venv .venv

# macOS/Linux
source .venv/bin/activate

# Windows
.venv\\Scripts\\activate
```

Install the backend dependencies:

```bash
pip install -r Backend/requirements.txt
```

Place the workforce dataset in the location configured by `config.py`. The loader supports `.csv`, `.xlsx`, and `.xls` files.

## Run the API

Run the application from the directory expected by the imports in `app.py`:

```bash
cd Backend
python app.py
```

The API runs on `http://localhost:5000` by default.

## Health checks

- `GET /` — Returns the project status and whether the dataset loaded successfully.
- `GET /api/health` — Returns API health, dataset status, and any dataset loading error.

## API route groups

- `/api/dashboard`
- `/api/employees`
- `/api/analytics`
- `/api/predict`
- `/api/chat`

## Dependencies

The backend uses Flask, Flask-CORS, pandas, NumPy, scikit-learn, joblib, and openpyxl. See `requirements.txt` for pinned versions.
