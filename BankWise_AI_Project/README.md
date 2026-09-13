# BankWise AI

## AI-Powered Banking Marketing Intelligence Platform

BankWise AI is a Cognizant hackathon project that helps banking marketing teams move from customer insight to compliant campaign delivery in one governed workflow. It combines customer analytics, explainable product recommendations, generative AI campaign copy, locally generated marketing creatives, compliance validation, human approval, multi-channel delivery, and campaign analytics.

The project is designed around a practical banking principle: AI can accelerate marketing decisions, but customer privacy, compliance controls, and human accountability remain part of the product workflow.

## Product Vision

Bank marketing teams often work across disconnected customer data, analytics, copywriting, creative production, compliance review, and delivery tools. BankWise AI brings those activities into a single workspace so teams can:

- Understand customers through a 360-degree view and behavioral segments.
- Identify relevant products using eligibility-aware machine learning recommendations.
- Generate personalized campaign content with Google Gemini.
- Produce consistent marketing banners locally with Pillow.
- Check campaign language and consent before activation.
- Require human review and approval before delivery.
- Deliver approved campaigns through email and simulated SMS workflows.
- Measure campaign outcomes and compare A/B variants.
- Preserve campaign, approval, creative, and delivery events in an audit trail.

## Hackathon Value Proposition

| Challenge                     | BankWise AI response                                                    |
| ----------------------------- | ----------------------------------------------------------------------- |
| Generic mass marketing        | Segment-aware, customer-level recommendations and copy                  |
| Slow campaign production      | GenAI-assisted copy and automated creative generation                   |
| Compliance risk               | Automated checks plus human-in-the-loop approval                        |
| Limited explainability        | Recommendation rationale and campaign context are visible to users      |
| Fragmented execution          | Insight, creation, approval, delivery, and analytics in one application |
| Weak operational traceability | SQLite-backed campaign, approval, creative, and delivery logs           |

## Core Capabilities

### Customer intelligence

- Customer 360 profiles with account, behavioral, digital-affinity, and retention-risk signals.
- Segment distributions and segment-level insights.
- Searchable and filterable customer and recommendation views.

### Explainable recommendations

- Feature engineering and customer profiling from banking datasets.
- Product eligibility checks before scoring.
- Product propensity and recommendation selection.
- Recommendation summaries and rationale exposed through the API and UI.

### GenAI campaign studio

- Customer and product context selection.
- Personalized subject lines and campaign copy.
- English, Hindi, and Marathi language support in the campaign workflow.
- Tone selection and A/B campaign variants.
- Editable campaign content before review.

### Creative studio

- Three creative options generated from campaign context.
- 1200 x 675 marketing banners created locally with Pillow.
- Creative regeneration and approval workflow.
- SHA-256 metadata and provenance records for generated assets.

### Compliance and governance

- Detection of prohibited or risky campaign language, including unsupported guarantees.
- Explicit consent confirmation in the compliance workflow.
- Review and approval actions recorded against campaign IDs.
- Privacy-oriented prompt construction that avoids sending raw customer identifiers to Gemini.

### Delivery and optimization

- Approved creative can be embedded in email delivery.
- Gmail SMTP delivery is supported when credentials are configured.
- SMS dispatch is represented through the application delivery workflow.
- Delivery logs, campaign analytics, and A/B testing views support optimization.

## Application Walkthrough

| Route              | Purpose                                                                           |
| ------------------ | --------------------------------------------------------------------------------- |
| `/`                | Executive dashboard with customer, recommendation, campaign, and performance KPIs |
| `/customers`       | Customer 360 search, profile details, and behavioral signals                      |
| `/segments`        | Segment distribution and customer intelligence                                    |
| `/recommendations` | Product recommendations, filters, summaries, and rationale                        |
| `/campaign-studio` | Generate and edit personalized campaign content                                   |
| `/creative-studio` | Generate, compare, and approve campaign banners                                   |
| `/compliance`      | Run compliance checks and submit campaigns for approval                           |
| `/delivery`        | Send approved campaigns through email or SMS workflow                             |
| `/analytics`       | Review campaign and delivery performance                                          |
| `/governance`      | Review audit and governance information                                           |

## Architecture

```text
                  +-----------------------------+
                  | React + Vite + TypeScript   |
                  | Tailwind CSS + Recharts     |
                  +-------------+---------------+
                                | Axios / REST
                                v
                  +-------------+---------------+
                  | FastAPI application         |
                  | CORS, routers, validation   |
                  +---+-----------+----------+---+
                      |           |          |
          +-----------+--+   +----+-----+  +-+----------------+
          | ML/data logic |   | Gemini   |  | SQLite audit DB |
          | CSV pipeline  |   | GenAI    |  | lifecycle logs  |
          +-----------+---+   +----+-----+  +-----------------+
                      |           |
                data/raw and   campaign copy
              data/processed   and variants
                                  |
                         +--------+---------+
                         | Pillow creatives |
                         | Gmail SMTP       |
                         +------------------+
```

### Technology stack

- **Frontend:** React 19, TypeScript, Vite, Tailwind CSS 4, React Router, Axios, Recharts, Lucide React.
- **Backend:** Python 3.10+, FastAPI, Pydantic, Uvicorn, python-dotenv.
- **Data and ML:** Pandas, NumPy, scikit-learn, CSV-based feature and campaign datasets.
- **Generative AI:** Google Gemini through `google-genai` and `google-generativeai`.
- **Creative generation:** Pillow.
- **Persistence:** SQLite for campaigns, approvals, deliveries, and creative assets.
- **Legacy/demo UI:** Streamlit entry point in `app.py` remains available for the original Python experience.

## Repository Structure

```text
.
├── backend/                 # FastAPI app and REST routers
│   ├── main.py
│   ├── routers/             # Customers, campaigns, creative, compliance, delivery, analytics...
│   └── services/            # Campaign and creative service helpers
├── frontend/                # React + Vite application
│   ├── src/pages/           # Product workflow screens
│   ├── src/components/      # Shared layout and UI components
│   └── src/services/api.ts  # Typed frontend API client
├── src/                     # Data, segmentation, recommendation, and campaign pipeline
├── data/
│   ├── raw/                 # Source datasets
│   ├── processed/           # Feature and analytics datasets consumed by the API
│   └── generated_creatives/ # Generated creative JSON metadata and assets
├── ui/                      # Original Streamlit screens and helpers
├── finora_db.py             # SQLite campaign lifecycle persistence
├── build_features.py        # Feature and processed-data build entry point
├── inspect_dataset.py       # Dataset inspection utility
├── app.py                   # Original Streamlit entry point
└── requirements.txt         # Python dependencies
```

## Quick Start

### Prerequisites

- Python 3.10, 3.11, or 3.12.
- Node.js 18 or newer and npm.
- A Google Gemini API key for live campaign generation.
- Optional Gmail account and app password for email delivery.

### 1. Clone the repository

```bash
git clone https://github.com/RohitKhobare/Effective_Bank_Marketing_Using_GenAI.git
cd Effective_Bank_Marketing_Using_GenAI
```

### 2. Configure the backend

Create a local `.env` file in the repository root. Never commit this file.

```dotenv
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-2.0-flash
GMAIL_ADDRESS=your_gmail_address
GMAIL_APP_PASSWORD=your_gmail_app_password
```

Only configure Gmail variables when testing real email delivery. A Gmail app password is required when two-step verification is enabled; do not use a normal account password.

Create a virtual environment and install dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
```

On macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 3. Start the FastAPI backend

From the repository root:

```bash
uvicorn backend.main:app --reload --port 8000
```

Useful endpoints:

- Health check: `http://localhost:8000/api/health`
- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

### 4. Start the React frontend

Open a second terminal:

```bash
cd frontend
npm install
npm run dev
```

Open `http://localhost:5173`. The Vite development server proxies `/api` requests to the FastAPI server on port 8000.

### 5. Optional: run the Streamlit application

The original Python UI can be started independently:

## Data Pipeline

The project uses a reproducible CSV-based pipeline:

1. Raw customer, transaction, product, holdings, and campaign interaction data is stored under `data/raw`.
2. Dataset loaders validate and normalize source data.
3. Feature engineering produces customer-level behavioral features.
4. Segmentation creates clusters, profiles, and final segment labels.
5. Product eligibility and scoring generate customer-product recommendations.
6. Campaign context and content datasets support the campaign workflow.
7. FastAPI routers read processed outputs for interactive screens.

To rebuild feature outputs from source data:

```bash
python build_features.py
```

For dataset diagnostics:

```bash
python inspect_dataset.py
```

## API Surface

All routers are available under both `/api/...` and the unprefixed route for compatibility. The primary frontend uses the `/api` prefix.

| Area            | Main endpoints                                                                               |
| --------------- | -------------------------------------------------------------------------------------------- |
| Health          | `GET /api/health`                                                                            |
| Customers       | `GET /api/customers`, `GET /api/customers/{customer_id}`                                     |
| Segments        | `GET /api/segments`                                                                          |
| Recommendations | `GET /api/recommendations`, `GET /api/recommendations/{customer_id}`                         |
| Campaigns       | `GET /api/campaigns/context`, `POST /api/campaigns/generate`, `GET /api/campaigns`           |
| Creative        | `POST /api/creative/generate`, `POST /api/creative/approve`                                  |
| Compliance      | `POST /api/compliance/check`                                                                 |
| Approval        | `POST /api/campaigns/{campaign_id}/review`, `POST /api/campaigns/{campaign_id}/approve`      |
| Delivery        | `POST /api/campaigns/{campaign_id}/send-email`, `POST /api/campaigns/{campaign_id}/send-sms` |
| Analytics       | `GET /api/analytics/summary`, `GET /api/analytics/campaigns`                                 |
| A/B testing     | `GET /api/ab-testing/analytics`, `GET /api/ab-testing/variants`                              |

The complete interactive contract is generated automatically by FastAPI at `/docs`.

## Governance and Privacy Design

- API keys and delivery credentials are loaded server-side from environment variables.
- Secrets are excluded from Git through `.gitignore`.
- Customer identifiers and raw sensitive attributes should not be included in prompts sent to the GenAI provider.
- Compliance checks are advisory controls and do not replace legal, regulatory, or business-owner review.
- Consent is explicitly confirmed in the workflow because the current demonstration dataset does not store channel consent fields.
- Campaign status changes, approvals, deliveries, and creative approvals are persisted for traceability.
- This repository uses synthetic or demonstration data only. Do not upload production customer data.

## Validation

Frontend build and lint commands:

```bash
cd frontend
npm run build
npm run lint
```

Backend smoke check:

```bash
python -c "from backend.main import app; print(app.title)"
```

With the backend running, verify:

```bash
curl http://localhost:8000/api/health
```

Expected response:

```json
{ "status": "ok", "service": "BankWise AI API" }
```

## GitHub Publishing and Hosting Notes

The source repository is intended to be published at:

https://github.com/RohitKhobare/Effective_Bank_Marketing_Using_GenAI

GitHub is suitable for storing and reviewing this project, but GitHub Pages only hosts static frontend files. It cannot run the FastAPI server, SQLite persistence, Gemini calls, or Gmail SMTP delivery. A complete live deployment therefore needs:

- **Frontend:** GitHub Pages, Azure Static Web Apps, or another static hosting service.
- **Backend:** Azure App Service, Azure Container Apps, Render, Railway, or another Python-capable service.
- **Data:** packaged demonstration CSVs or managed storage/database for a production deployment.
- **Secrets:** platform-managed environment variables or a secret manager, never repository files.

For a GitHub-only static demo, the frontend must be adapted to use a publicly hosted backend URL instead of the local Vite proxy. For the full hackathon experience, deploy the backend and frontend separately and configure CORS for the deployed frontend origin.

## Security Checklist Before Public Upload

- Confirm `.env`, API keys, Gmail credentials, and app passwords are not tracked.
- Remove any real customer records or personally identifiable information.
- Review generated creative metadata and logs for sensitive values.
- Rotate any credential that may have been exposed during development.
- Configure production CORS to allow only the deployed frontend domain.
- Use HTTPS and platform-managed secrets in deployment.

## Team and Project Context

**Project:** BankWise AI
**Event:** Cognizant Hackathon
**Focus:** Responsible GenAI for personalized banking marketing
**Repository:** [Effective_Bank_Marketing_Using_GenAI](https://github.com/RohitKhobare/Effective_Bank_Marketing_Using_GenAI)

## License

No license file is currently included. Add a license before accepting external contributions or reusing the project commercially.

## Team Members

1. Rohit Khobare
2. Shreya Berlikar
3. Amogh Waskar
4. Vasudha Patil
5. Sneha Dhandhe
6. Omkar Bagwale
7. Abhijeet Wagurde
   8.Pruthu Unhale
