# Project Setup Guide

This project is split into three repositories:

| Repository   | Description                                             |
| ------------ | ------------------------------------------------------- |
| `frontend`   | Web application (runs on `localhost:3000`)              |
| `backend`    | NestJS API with Prisma ORM                              |
| `deployment` | Docker Compose setup for PostgreSQL, Redis and Keycloak |

## Prerequisites

Make sure the following are installed on your machine:

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/) and npm
- [Docker](https://www.docker.com/) with Docker Compose

## Getting Started

Follow the steps **in order**. Use a separate terminal for each long-running process (backend and frontend).

### 1. Clone the repositories

Clone all three repositories into the same parent folder:

```bash
git clone https://github.com/DylanBOLLARO/oidc-template-frontend
git clone https://github.com/DylanBOLLARO/oidc-template-backend
git clone https://github.com/DylanBOLLARO/oidc-template-deployment
```

### 2. Start the infrastructure (PostgreSQL, Redis, Keycloak)

```bash
docker compose up -d --force-recreate --build --remove-orphans
```

Check that the containers are running:

```bash
docker compose ps
```

### 3. Start the backend

```bash
npm i && npx prisma db push --force-reset && npx prisma generate && npm run dev
```

> ⚠️ **Warning:** `--force-reset` drops and recreates the database schema. All existing data will be lost. Only use it in local development.

### 4. Start the frontend

In a new terminal:

```bash
npm i && npm run dev
```

## Verify the Setup

1. Open [http://localhost:3000](http://localhost:3000).
2. Click **Login** in the top-left corner.
3. You will be redirected to **Keycloak**. Enter the credentials below.
4. Once logged in, you should be redirected back to the app.

### Test credentials

| Field    | Value                 |
| -------- | --------------------- |
| Email    | `jdoe@example.com`    |
| Password | `randompassword12345` |

> These credentials are for **local development only**. Never use them in production.

### Expected result

After logging in, you should see **"John Doe"** and a **Logout** button in the top-left corner. This confirms that the whole workflow (frontend → Keycloak → backend → database) is working correctly.

## Troubleshooting

| Problem                         | Possible fix                                                                              |
| ------------------------------- | ----------------------------------------------------------------------------------------- |
| Keycloak login page not loading | Wait a few seconds after starting Docker, Keycloak can take time to boot (10-30 seconds). |
