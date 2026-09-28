# StringTracker: Guitar Inventory System

A full-stack **Next.js + TypeScript** web app for guitar players and small shops to manage their guitar inventory. Each user gets a private collection they can browse, filter, edit and analyse. The app is containerised and was deployed to **AWS ECS** over HTTPS.

## Features

- **Accounts.** Register and sign in with NextAuth (JWT sessions, bcrypt-hashed passwords). Every page except the landing page is protected, and each user only ever sees their own guitars.
- **Full CRUD.** Add, update and delete guitars with detailed attributes, validated server-side with `class-validator`.
- **Filtering.** By manufacturer, type, condition, string count and price.
- **Price analytics.** Statistics and category highlights across the collection, answering what it's *worth*, not just what's in it.
- **REST API.** Next.js route handlers under `/api/guitars` and `/api/auth`, backed by a service layer (`GuitarService`, `BrandService`).

## Architecture

```
Next.js (App Router, React 19)
 ├─ UI pages ─────────────── protected by NextAuth session
 ├─ /api/guitars, /api/auth ─ route handlers
 └─ services ──── TypeORM ──── PostgreSQL
```

- **Data layer:** TypeORM entities with versioned migrations (`npm run migration:*`)
- **Deployment:** multi-stage Dockerfile running as a non-root user, `docker-compose` with PostgreSQL for local production-like runs, and a `deploy.sh` script that pushes the image to Amazon ECR and rolls out the ECS service
- **Tests:** Jest + React Testing Library

## Tech stack

`Next.js 15` `React 19` `TypeScript` `NextAuth` `TypeORM` `PostgreSQL` `Tailwind CSS` `Jest` `Docker` `AWS ECS / ECR`

## Getting started

### Prerequisites
- Node.js 18.17 or later
- Docker (optional, for the PostgreSQL setup)

### Run locally

```bash
git clone https://github.com/CostinCJ/StringTracker.git
cd StringTracker
npm install
cp .env.example .env.local   # set your PostgreSQL credentials
set -a; . ./.env.local; set +a   # load them for the migration CLI
npm run migration:run
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

### Run with Docker (app + PostgreSQL)

```bash
cp .env.example .env          # set POSTGRES_PASSWORD
docker compose up --build
```

### Tests

```bash
npm test
```
