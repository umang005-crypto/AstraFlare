# AstraFlare

AstraFlare is a full-stack thermal anomaly classification platform that combines satellite hotspot data, machine learning, database verification, and a responsive dashboard.

It analyzes thermal activity and distinguishes between:

- Industrial fire
- Agricultural burning
- Wildfire
- Persistent thermal sources
- Other detected heat sources

## Features

- Thermal anomaly ingestion and storage
- Fire and industrial-risk classification
- Endangered-industry proximity analysis
- Confidence and danger-level estimation
- Prediction verification against stored anomaly data
- REST APIs for predictions and analytics
- Interactive React dashboard
- Authority report and dispatch workflow
- PostgreSQL-backed prediction history
- Isolated Python ML microservice
- Docker support for the backend database

## Architecture

```text
Satellite / NASA FIRMS hotspot data
                |
                v
TypeScript Backend - Ingestion and REST APIs
                |
                v
PostgreSQL - Thermal anomalies and verified predictions
                |
                v
Python FastAPI ML Service - Classification and risk analysis
                |
                v
TypeScript Verification Engine
                |
                v
React Frontend - Dashboard and incident reports
```

## Project Structure

```text
AstraFlare/
├── frontend/       React and TypeScript dashboard
├── backend/        Express, TypeScript, PostgreSQL API
├── ml/             Python ML and FastAPI service
├── walkthrough.md  Implementation and verification notes
└── .gitignore
```

## Technology Stack


## Research References 

AstraFlare's research methodology is informed by published studies on satellite-based active fire detection, industrial thermal anomaly identification, and VIIRS/MODIS remote sensing.

### 1. Industrial Heat Source Identification

**Paper:** Identifying Industrial Heat Sources Using Time-Series of the VIIRS Nightfire Product with an Object-Oriented Approach (2018)

- Authors: Yongxue Liu et al.
- Journal: Remote Sensing of Environment
- DOI: https://doi.org/10.1016/j.rse.2017.10.019

**Research Relevance:**
- Understanding persistent industrial thermal signatures.
- Analyzing spatial and temporal patterns of thermal anomalies.
- Distinguishing industrial heat sources from biomass burning.

### 2. Satellite-Based Active Fire Detection

**Paper:** The New VIIRS 375 m Active Fire Detection Data Product: Algorithm Description and Initial Assessment (2014)

- Authors: Wilfrid Schroeder, Patricia Oliva, Louis Giglio, Ivan A. Csiszar.
- Journal: Remote Sensing of Environment
- DOI: https://doi.org/10.1016/j.rse.2013.12.008

**Research Relevance:**
- Understanding VIIRS active fire detection algorithms.
- Studying thermal infrared features and contextual detection methods.
- Exploring satellite-based detection of biomass burning and other thermal anomalies.

### Application in AstraFlare

The reviewed literature provides a research foundation for:

- Satellite-based thermal anomaly analysis using NASA FIRMS data.
- Industrial heat-source and wildfire classification.
- Spatiotemporal feature analysis and geospatial risk assessment.
- Reducing false positives and improving anomaly verification.

**Note:** These publications are research references, not claims of direct implementation of their original algorithms.

### Frontend

- React
- TypeScript
- Vite
- React Router
- CSS

### Backend

- Node.js
- Express
- TypeScript
- PostgreSQL
- Node Cron
- Docker Compose

### Machine Learning

- Python
- FastAPI
- Pydantic
- NumPy
- scikit-learn
- Uvicorn

## Prerequisites

Install the following tools:

- Node.js 18 or newer
- npm
- Python 3.10 or newer
- PostgreSQL
- Docker and Docker Compose, optional but recommended

## Installation

```bash
git clone https://github.com/umang005-crypto/AstraFlare.git
cd AstraFlare
```

## Backend Setup

### 1. Install dependencies

```bash
cd backend
npm install
```

### 2. Configure environment variables

Create a `.env` file from the provided example:

```bash
cp .env.example .env
```

Update the database and application variables:

```env
PORT=5000
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/astraflare_db
```

### 3. Start PostgreSQL with Docker

```bash
docker compose up -d
```

### 4. Start the backend

For development:

```bash
npm run dev
```

For a compiled build:

```bash
npm run build
npm start
```

The backend normally runs on:

```text
http://localhost:5000
```

Health endpoint:

```text
http://localhost:5000/api/health
```

## ML Service Setup

### 1. Open the ML directory

```bash
cd ../ml
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Linux or macOS:

```bash
source .venv/bin/activate
```

Activate it on Windows:

```powershell
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Start the ML API

```bash
python service.py
```

The ML service normally runs on:

```text
http://localhost:8001
```

Health endpoint:

```text
http://localhost:8001/health
```

## Frontend Setup

Open a separate terminal:

```bash
cd frontend
npm install
```

Create an environment file:

```bash
cp .env.example .env
```

Configure the backend URL:

```env
VITE_API_URL=http://localhost:5000
```

Start the development server:

```bash
npm run dev
```

The frontend is usually available at:

```text
http://localhost:5173
```

## Running the Complete System

Start the services in separate terminals:

```bash
# Terminal 1 - Database
cd backend
docker compose up -d
```

```bash
# Terminal 2 - Backend
cd backend
npm run dev
```

```bash
# Terminal 3 - ML service
cd ml
python service.py
```

```bash
# Terminal 4 - Frontend
cd frontend
npm run dev
```

## Main API Endpoints

### Backend

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/health` | Check backend and database health |
| `GET` | `/api/predictions` | Retrieve verified predictions |
| `GET` | `/api/predictions/:id` | Retrieve a prediction report |
| `GET` | `/api/predictions/stats` | Retrieve prediction statistics |
| `POST` | `/api/predictions/submit` | Submit and verify one prediction |
| `POST` | `/api/predictions/batch` | Submit and verify multiple predictions |

### ML Service

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/health` | Check ML service health |
| `POST` | `/predict` | Generate a prediction |
| `POST` | `/predict-and-submit` | Generate and submit a verified prediction |
| `POST` | `/sync-active-anomalies` | Process active stored anomalies |

## Prediction Flow

1. Thermal anomaly data is ingested into PostgreSQL.
2. The ML service receives anomaly features.
3. The model classifies the event and calculates risk estimates.
4. The backend validates the prediction structure.
5. Coordinates and anomaly IDs are checked against PostgreSQL.
6. Physical consistency rules are applied.
7. Verified predictions are stored in the database.
8. The frontend retrieves and displays the verified results.

## Development Scripts

### Backend

```bash
npm run dev       # Start with hot reload
npm run build     # Compile TypeScript
npm start         # Start the application
npm run ingest    # Run the ingestion CLI
```

### Frontend

```bash
npm run dev       # Start Vite development server
npm run build     # Build the production bundle
npm run lint      # Run ESLint
npm run preview   # Preview the production build
```

### ML

```bash
python train.py                 # Train the model
python prepare_experiment_dataset.py
python build_100_event_demo.py
python service.py               # Start the API service
python service.py sync          # Sync active anomalies
```

## Testing

Run ML tests from the `ml` directory:

```bash
pytest
```

Run a frontend or backend test command only if one has been added to the relevant package scripts.

## Environmental and Safety Notice

AstraFlare is a decision-support project. Predictions may contain false positives or false negatives.

- Always verify detected events through authoritative sources.
- Require human review before emergency dispatch.
- Do not interpret confidence percentages as guaranteed outcomes.
- Avoid using demonstration data for real-world emergency decisions.
- Apply appropriate access controls before operational deployment.

## Recommended Improvements

- Add a complete root setup and deployment guide
- Add automated CI/CD workflows
- Add frontend and backend test suites
- Add model performance metrics and dataset documentation
- Add API authentication and role-based authorization
- Add database migrations and seed scripts
- Add structured logging and monitoring
- Add rate limiting and request validation
- Add model-version tracking
- Add an open-source license
- Document satellite-data sources and update schedules

## Contributing

1. Fork the repository.
2. Create a feature branch:

   ```bash
   git checkout -b feature/my-feature
   ```

3. Commit your changes:

   ```bash
   git commit -m "Add my feature"
   ```

4. Push the branch:

   ```bash
   git push origin feature/my-feature
   ```

5. Open a pull request.

## License

A license has not yet been specified. Do not treat the project as approved for unrestricted commercial or production use until a license is added.
