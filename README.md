# FreelancerCRM

**FreelancerCRM** is an advanced-level Minimum Viable Product (MVP) version of a modern CRM web application built for learning purposes and upskill reasons, and includes an overview of client relationships, activities, and communication — all presented in a user-friendly, responsive dashboard with a drag-and-drop Kanban interface.

---

## Try the App

Test the live application here:

* **Live App (Vercel)**
  [https://freelancercrm-deployment.vercel.app](https://freelancercrm-deployment.vercel.app)

* **API Health Check (Heroku)**
  [https://freelancercrm-backend-838d0cf51a08.herokuapp.com/api/health](https://freelancercrm-backend-838d0cf51a08.herokuapp.com/api/health)

Live application availability (YES/NO): **YES**

---

## About the Application

I built FreelancerCRM to challenge myself, so I can enhance my development competency by creating a complete full-stack solo project while also exploring different technologies not previously well known to me.

The application has a Kanban board UI built with React/Next.js and Tailwind, and a secure Node/Express back-end with both REST and GraphQL endpoints backed by PostgreSQL.

I set up JWT authentication, CI/CD with GitHub Actions, and deployed the front-end to Vercel and the back-end to Heroku. I also experimented with infrastructure-as-code using Terraform and lightweight Kubernetes (K3s) although I wouldn’t call myself proficient there yet, but it gave me valuable exposure to how modern apps can be deployed and scaled.

The application contains the following features:

* Manage contacts with fields for name, email, company, status, note, and phone number.
* Organize client workflows using a visual Kanban board with drag-and-drop functionality.
* Includes search, filter, generate and reset contacts buttons.
* Log and track activities linked to each contact.
* User authentication (registration, login, protected routes).
* Profile page with total amount of contacts and activities, and email/password update.
* Supports dark/light mode with a toggle button and your system settings.
* Mobile-friendly and responsive design.

Upcoming Features/Fixes:

* None currently

---

## Technologies

**Frontend**

* Next.js (TypeScript)
* React
* Apollo Client
* Tailwind CSS

**Backend**

* Express.js (Node.js/TypeScript)
* REST
* GraphQL (Apollo Server)
* Prisma ORM

**Database**

* PostgreSQL

**Authentication**

* JWT

**DevOps**

* Docker
* Kubernetes
* Terraform
* GitHub Actions
* Vercel
* Heroku

**Testing**

* Jest
* Cypress

**Tools**

* VS Code
* AI (GPT-4)
* Framework Documentation
* YouTube Guides
* StackOverflow

---

## View / Run the Source Code

```bash
git clone https://github.com/Enock97/FreelancerCRM.git
```

---

## Local Setup

This is a Next.js project bootstrapped with `create-next-app`.

### Getting Started

First, run the development server:

```bash
npm run dev
```

or

```bash
yarn dev
pnpm dev
bun dev
```

Open:

```
http://localhost:3000
```

You can start editing the page by modifying:

```
app/page.tsx
```

The page auto-updates as you edit the file.

This project uses `next/font` to automatically optimize and load the Geist font family.

---

## Learn More

To learn more about Next.js, take a look at the following resources:

Next.js Documentation
[https://nextjs.org/docs](https://nextjs.org/docs)

Learn Next.js
[https://nextjs.org/learn](https://nextjs.org/learn)

Next.js GitHub repository
[https://github.com/vercel/next.js/](https://github.com/vercel/next.js/)

---

## Local Development Setup

### Prerequisites

* Node.js v18+
* Docker + Docker Compose
* Optional: `kubectl` and a local Kubernetes cluster (minikube or kind)
* Optional: Terraform configurations

---

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/FreelancerCRM.git
cd FreelancerCRM
```

---

### 2. Activate the database

There are two main options.

#### Option A: Local development with SQLite

```bash
cd backend
cp .env.example .env
npm install
npx prisma generate
npx prisma migrate dev --name init
```

This sets up the SQLite database and runs all Prisma migrations.

---

#### Option B: PostgreSQL via Docker or Kubernetes

If using Docker or Kubernetes, PostgreSQL will start as a container or pod automatically.

Configure `.env` with the correct database URL:

```
DATABASE_URL="postgresql://user:password@localhost:5432/freelancercrm"
```

Then run:

```bash
npx prisma generate
npx prisma migrate deploy
```

Use `migrate deploy` instead of `migrate dev` in production-like environments.

---

### 3. Start the backend server

```bash
npm run dev
```

API runs on:

```
http://localhost:4000
```

---

### 4. Start the frontend app

Open a new terminal.

```bash
cd frontend
npm install
npm run dev
```

Frontend runs on:

```
http://localhost:3000
```

---

### 5. Optional: Docker Compose setup

```bash
docker-compose up --build
```

You must have a valid `docker-compose.yml` that wires up backend, frontend, and Postgres.

---

### 6. Optional: Kubernetes deployment

```bash
kubectl apply -f k8s/
```

This sets up the backend, frontend, and database in a Kubernetes cluster.

You must ensure your local cluster supports persistent volumes.

---

## Infrastructure and CI/CD (Terraform + GitHub Actions)

This repo ships with reproducible infrastructure-as-code.

Everything that Vercel, Heroku and the CI pipeline need lives in:

* `/infra` (Terraform)
* `.github/workflows/ci-tests-deploy-terraform.yml` (GitHub Actions)

| Layer            | Technology           | Local Usage                                               |
| ---------------- | -------------------- | --------------------------------------------------------- |
| Frontend hosting | Vercel provider      | `terraform apply` creates or updates the Vercel project   |
| API + database   | Heroku provider      | Same `terraform apply` provisions Heroku app and Postgres |
| Remote state     | Terraform Cloud Free | State stored remotely                                     |
| CI/CD            | GitHub Actions       | Tests → backend deploy → Terraform apply                  |

---

### Quick start

Install Terraform (>= 1.7)

```bash
cd infra
```

Login to Terraform Cloud

```bash
terraform login
```

Set required secrets:

```bash
export TF_VAR_heroku_email="you@example.com"
export TF_VAR_heroku_api_key="heroku-******"
export TF_VAR_vercel_token="vcst_******"
export TF_VAR_jwt_secret="super-secret-jwt-string"
```

Initialize and deploy:

```bash
terraform init
terraform apply
```

---

### After setup

* `npm test` and `npm run build` run automatically in the GitHub workflow
* Pushing to **main** triggers:

  * Tests
  * Backend deployment
  * Terraform apply
  * Frontend deployment

To create a staging environment:

```bash
terraform workspace new staging
terraform apply
```

---

### Changing cloud resources

Heroku stack:

```
stack = "heroku-24"
```

Database plan:

```
plan = "heroku-postgresql:essential-1"
```

Environment variables can be added in:

```
heroku_app_config_association.api_env
```

---

### Disabling duplicate Heroku builds

Because GitHub Actions already deploys the slug, disable **Automatic Deploys** in the Heroku dashboard to avoid duplicate builds.

---

### Why Terraform

One command:

```
terraform apply
```

can recreate the full Vercel + Heroku infrastructure, including CI configuration and secrets.

This allows anyone to fork the repository and spin up an identical environment quickly.
