# Project Setup

This project uses Docker to run a Next.js frontend, a Strapi backend, and a PostgreSQL database.

## Docker images
- docker pull ghcr.io/vuminhle1997/felix-nguyen-strapi-next:v1.0.5
- docker pull ghcr.io/vuminhle1997/felix-nguyen-strapi-strapi:v.1.0.5

## Environment Variables

Create a single `.env` file in the root directory. Both Next.js and Strapi, as well as the database container, will read from it.

### Required Environment Variables

```dotenv
# --- Database Configuration ---
DATABASE_CLIENT=postgres
DATABASE_HOST=db
DATABASE_PORT=5432
DATABASE_NAME=strapi
DATABASE_USERNAME=strapi
DATABASE_PASSWORD=strapi

# --- Strapi Backend Configuration ---
HOST=0.0.0.0
PORT=1337
# Generate these secrets securely (e.g. using `openssl rand -base64 32`)
APP_KEYS="toBeModified1,toBeModified2"
API_TOKEN_SALT=tobemodified
ADMIN_JWT_SECRET=tobemodified
TRANSFER_TOKEN_SALT=tobemodified
JWT_SECRET=tobemodified

# --- Next.js Frontend Configuration ---
# Required for Next.js to communicate with Strapi internally in Docker
NEXT_PUBLIC_API_URL=http://strapi:1337
# Base URL of the site
NEXT_PUBLIC_BASE_URL=http://localhost:3000
# Token from Strapi for Next.js to fetch data
STRAPI_API_TOKEN=your_token_here
# Optional: Google Analytics and other services
NEXT_PUBLIC_GOOGLE_ANALYTICS_ID=
PREVIEW_SECRET=preview_secret
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=user
SMTP_PASSWORD=password
SMTP_FROM=no-reply@example.com
COMPANY_EMAIL=contact@example.com
RECAPTCHA_SITE_KEY=
RECAPTCHA_SECRET_KEY=
```

## Running with Docker Compose

1. Ensure **Docker** and **Docker Compose** are installed on your machine.
2. In the root directory, create the `.env` file and populate it with the variables mentioned above. Make sure to generate secure secrets for production!
3. Start all services using Docker Compose:

```bash
docker-compose up -d
```

- **Next.js Frontend** will be available at: http://localhost:3000
- **Strapi Backend** will be available at: http://localhost:1337 (Admin panel at http://localhost:1337/admin)
- **PostgreSQL Database** is running on port `5432`

### Useful Docker Commands

- **Check logs:**
  ```bash
  docker-compose logs -f
  ```
  Or for a specific service: `docker-compose logs -f next`

- **Rebuild images and restart:**
  If you change package dependencies or Dockerfiles:
  ```bash
  docker-compose up -d --build
  ```

- **Stop all services:**
  ```bash
  docker-compose down
  ```

## CI/CD Pipeline

A GitHub Actions workflow is provided (`.github/workflows/docker-publish.yml`). It automatically builds and pushes the Next.js and Strapi Docker images to the GitHub Container Registry (`ghcr.io`).

**Trigger:** The pipeline is triggered whenever a tag starting with `v` is pushed to the repository (e.g., `git tag v1.0.0 && git push origin v1.0.0`).
