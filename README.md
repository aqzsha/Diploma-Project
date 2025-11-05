# HealthScan — Multiplatform Product Health Analyzer  
*(Android + Web + Go)*

> Scan food or cosmetic products, detect potential health risks, and find safer substitutes — all in one ecosystem.

---

## 📖 Overview

**HealthScan** is a cross-platform system that helps users analyze whether a product suits their personal health conditions.  
Users can scan a barcode or take a picture of a label — the system extracts ingredients, normalizes their names, checks known risks (WHO, research articles, internal KB), and returns a detailed analysis with recommendations and evidence sources.

---

## ✨ Key Features

- 📷 **Scan Products** – via barcode or label image (OCR).  
- 🧠 **Ingredient Normalization** – unify names and synonyms.  
- 🌍 **Knowledge Integration** – internal KB + WHO + scientific articles.  
- ⚠️ **Risk Classification** – Dangerous / Questionable / Potential + citations.  
- 👤 **Personalization** – adjust results by user conditions/diseases.  
- 🔄 **Substitute Finder** – suggests safer alternatives.  
- 📱 **Cross-Platform** – Android app + React web client.  
- 🧩 **Microservice Backend** – modular Go services for scalability.

---

## 🧱 Architecture

```text
[ Android App (Kotlin) ]         [ Web (React) ]
             |                          |
             |      HTTPS / WebSocket   |
             v                          v
                 [ API Gateway (Go) ]
                          |
        +-----------------+-----------------+
        |                 |                 |
 [ Products Svc ]   [ Analysis Svc ]   [ User Svc ]
        |                 |                 |
     PostgreSQL         Redis           PostgreSQL
        |                 |
     MinIO (images)     RabbitMQ  <--- events --->  [ Notifications Svc ]
        |
     OCR Worker (Tesseract / ML Kit)
        |
 External Sources (WHO / Articles) → fetcher + cache
🛠 Technologies
Frontend (Web)
React + TypeScript + Vite

Ant Design / Tailwind

Zustand / TanStack Query

i18next (EN / RU / KK)

Mobile (Android)
Kotlin + Jetpack (Navigation, ViewModel)

CameraX / ML Kit (OCR)

Retrofit + Room

Material Components

Backend (Go)
Gin / Chi

PostgreSQL (sqlx / GORM)

Redis (cache, locks)

RabbitMQ (async events)

MinIO (object storage)

OpenTelemetry, Prometheus, Grafana, Graylog

Docker Compose / Kubernetes (deployment)

📂 Repository Structure
text
Копировать код
.
├─ mobile/                  # Android app (Kotlin)
├─ web/                     # React web app
├─ services/
│  ├─ api-gateway/          # entrypoint for clients
│  ├─ products/             # product scanning, OCR, metadata
│  ├─ analysis/             # risk classification, normalization
│  ├─ users/                # profiles, health conditions
│  └─ notifications/        # push/email reports
├─ infra/
│  ├─ docker/               # Dockerfiles
│  ├─ compose/              # docker-compose.yml + .env.example
│  └─ k8s/                  # Kubernetes manifests
├─ docs/                    # API specs, architecture, diagrams
└─ README.md
⚡ Quick Start (Docker Compose)
bash
Копировать код
cp infra/compose/.env.example infra/compose/.env
docker compose -f infra/compose/docker-compose.yml up -d --build
make migrate     # apply DB migrations if configured
make run         # start backend services
⚙️ Environment Variables
Example .env:

env
Копировать код
POSTGRES_USER=healthscan
POSTGRES_PASSWORD=devpass
POSTGRES_DB=healthscan
REDIS_HOST=redis
RABBITMQ_HOST=rabbitmq
MINIO_ENDPOINT=minio:9000
JWT_SECRET=dev-secret
API_BASE_URL=http://localhost:8080
WEB_URL=http://localhost:5173
ANDROID_API_URL=http://10.0.2.2:8080
🚀 Running Individually
Android
bash
Копировать код
# Open in Android Studio
# Ensure API URL = http://10.0.2.2:8080
Web
bash
Копировать код
cd web
cp .env.example .env
npm install
npm run dev
Backend
bash
Копировать код
cd services/api-gateway
cp .env.example .env
go run ./cmd/api
🔗 API Overview
Endpoint	Description
POST /v1/scan/barcode	Scan by barcode
POST /v1/scan/label	Upload image for OCR
GET /v1/products/{id}	Product details
POST /v1/analyze	Risk analysis + conditions
GET /v1/substitutes	Find analogues
GET /v1/users/me	User profile
POST /v1/feedback/report	Report issue

🧩 Data & Storage
PostgreSQL – products, users, history

Redis – cache, rate-limit, session storage

MinIO – image/object storage

RabbitMQ – async events for OCR/notifications

WHO / KB / Articles – versioned and cached sources

🌍 Localization (i18n)
Web: i18next JSON (/web/src/i18n/{en,ru,kk}.json)

Android: strings.xml

Backend: returns keys for translation consistency.

🔒 Security
JWT authentication

Input validation (OpenAPI + backend)

File upload limits, MIME filtering

Rate limiting and idempotency

Secrets managed via environment variables

📊 Monitoring & Logging
Prometheus + Grafana dashboards

OpenTelemetry tracing

Graylog / ELK centralized logs

Request correlation via traceId

🧪 Testing & Code Quality
Go – go test ./...

React – Vitest / Cypress

Android – JUnit, Espresso

Linters: golangci-lint, ESLint, ktlint

Pre-commit hooks via husky/lefthook

⚙️ CI/CD
GitHub Actions or GitLab CI

Lint → Test → Build → Deploy

Docker image tagging (app:sha, :latest)

Helm/Kustomize for K8s deploy

Database migrations as separate step

🗓 Roadmap
 Advanced OCR model fine-tuning

 Personalized substitution recommendations

 Offline mode (Android)

 More product categories (vitamins, cosmetics)

 Notifications for recalled/updated products

🤝 Contribution
Fork the repo

Create a branch: feat/your-feature

Commit with Conventional Commits (feat:, fix:...)

Submit a pull request

