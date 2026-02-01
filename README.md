# Chemical Equipment Parameter Visualizer

A hybrid application that runs as both a web app and a desktop app. Upload CSV files containing chemical equipment data (name, type, flowrate, pressure, temperature), view summary statistics and charts, and keep a history of the last five uploads. The same Django API powers both the React web interface and the PyQt5 desktop client.

## What’s in the repo

- **backend** — Django + Django REST Framework. Handles CSV uploads, stores the last five datasets in SQLite, exposes summary and history APIs, and serves PDF reports. Basic authentication is required for all API access.
- **frontend** — React app with Chart.js for tables and charts. Uses the same API as the desktop client.
- **desktop** — PyQt5 app with Matplotlib for the same visualizations (bar chart for parameter averages, pie chart for equipment type distribution). Connects to the same backend.
- **sample_equipment_data.csv** — Sample CSV at the project root for testing and demos. Columns: Equipment Name, Type, Flowrate, Pressure, Temperature.

## Prerequisites

- Python 3.10+ (for backend and desktop)
- Node.js 18+ and npm (for web frontend)
- Git

## Backend setup

From the project root:

```bash
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py create_demo_user
python manage.py runserver
```

The API will be at `http://127.0.0.1:8000`. The demo user is **admin** / **admin**. For production, create a real user with `python manage.py createsuperuser` and do not rely on the demo account.

## Web frontend setup

In a new terminal, from the project root:

```bash
cd frontend
npm install
npm run dev
```

The app will open at `http://localhost:3000`. It proxies `/api` to the Django backend, so the backend must be running. Sign in with the same credentials you use for the API (e.g. admin / admin), then upload a CSV or pick one of the recent uploads to see the table, summary, and Chart.js visualizations. You can download a PDF report from the dataset view.

## Desktop app setup

With the backend already running:

```bash
cd desktop
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python main.py
```

Sign in with your API credentials. Use “Upload CSV file” to send a file to the backend, then double‑click an item in “Recent uploads” to open the detail view with summary, Matplotlib bar and pie charts, and the data table. “Download PDF report” saves the report via the same API as the web app.

If the backend is not on `http://127.0.0.1:8000`, set the environment variable before running the desktop app:

```bash
export EQUIPMENT_API_URL=http://your-backend-host:8000
python main.py
```

## Quick test flow

1. Start the backend and create the demo user (see above).
2. Start the web app, sign in as admin / admin, upload `sample_equipment_data.csv` from the project root, and check the table and charts.
3. Start the desktop app, sign in with the same user, upload the same CSV (or any CSV with the required columns), and confirm you see the same summary and charts.
4. In either client, open a dataset and use “Download PDF report” to get the PDF.

## CSV format

The backend expects a CSV with these column names (order can vary):

- Equipment Name  
- Type  
- Flowrate  
- Pressure  
- Temperature  

Flowrate, Pressure, and Temperature should be numeric. The sample file at the project root (`sample_equipment_data.csv`) matches this format.

## Optional: deploy the web version

Build the frontend:

```bash
cd frontend
npm run build
```

Serve the `frontend/dist` folder with any static file server, and point its `/api` path to your deployed Django backend (e.g. via reverse proxy). Ensure CORS and authentication are configured correctly for your domain.

## License and contact

This project was built as an intern screening task. Use and adapt it as needed for your own demos or submissions.
