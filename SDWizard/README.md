[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](../LICENSE)

# SD Creation Wizard Docker Deployment Guide

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Repository Structure](#repository-structure)
- [API Service](#api-service)
  - [Build & Run](#build--run)
  - [Production Best Practices](#production-best-practices)
- [Frontend Service](#frontend-service)
  - [Build & Smoke Test](#build--smoke-test)
  - [Docker Compose Deployment](#docker-compose-deployment)
- [Push to Registry](#push-to-registry)
- [Verification & Cleanup](#verification--cleanup)

## Introduction

The SD Creation Wizard simplifies the process of building and deploying the Self-Description microservices and frontend. This document describes Docker-based setup for both the API and frontend components. (e.g., `/getAvailableShapes`, `/getJSON?name=<shape>`)

## Prerequisites

Ensure you have the following installed:

- **Docker Engine** (>= v20)
- **Docker Compose** (>= v1.27) (required for frontend compose deployments)
- **Git CLI**
- *(Optional)* Java 21 JRE (for local testing of the API)
- *(Optional)* Angular CLI (`npm install -g @angular/cli`)

## Repository Structure

```bash
sd-creation-wizard-api/
├── deployment/
│   ├── Dockerfile
│   └── .dockerignore
└── src/
    └── ...

sd-creation-wizard-frontend/
├── deployment/
│   └── docker-compose.yml
├── Dockerfile
├── nginx.conf
└── implementation/
    └── ...
```

## API Service

### Build & Run

```bash
# Navigate to the API deployment directory
mkdir -p sd-creation-wizard-api/deployment
cd sd-creation-wizard-api/deployment

# Build the Docker image
docker build -t sd-creation-wizard-api:latest -f Dockerfile ..

# Run the container
docker run -d --name sd-wizard-api \
  -p 8080:8080 \
  sd-creation-wizard-api:latest

# Verify the service
curl http://localhost:8080/getAvailableShapes
```

### Production Best Practices

- **Smaller Base Image**: Use an Alpine-based JRE image (`eclipse-temurin:21-jre-alpine`).
- **Non‑Root User**: Create and switch to a non-root user for improved security.
- **Healthchecks**: Add a `HEALTHCHECK` directive in the `Dockerfile` to monitor container health.
- **.dockerignore**: Exclude unnecessary files (e.g., `target/`, `.git/`) to reduce build context.

## Frontend Service

### Build & Smoke Test

```bash
# Navigate to the frontend root directory
mkdir sd-creation-wizard-frontend
cd sd-creation-wizard-frontend

# Build the Docker image
docker build -t sd-creation-wizard-frontend:latest .

# (Optional) Standalone smoke test
docker run -d --name sd-wizard-frontend \
  -p 80:80 \
  sd-creation-wizard-frontend:latest
```

Access the UI at [http://localhost](http://localhost).

### Docker Compose Deployment

Configure `deployment/docker-compose.yml`:

```yaml
version: '3.8'
services:
  api:
    image: <REGISTRY>/sd-creation-wizard-api:latest
    ports:
      - '8080:8080'

  frontend:
    image: <REGISTRY>/sd-creation-wizard-frontend:latest
    ports:
      - '80:80'
    depends_on:
      - api
```

Launch the stack:

```bash
cd sd-creation-wizard-frontend/deployment
docker-compose up -d
```

## Push to Registry

```bash
# Tag images
docker tag sd-creation-wizard-api:latest <REGISTRY>/sd-creation-wizard-api:latest
docker tag sd-creation-wizard-frontend:latest <REGISTRY>/sd-creation-wizard-frontend:latest

# Push to registry
docker push <REGISTRY>/sd-creation-wizard-api:latest
docker push <REGISTRY>/sd-creation-wizard-frontend:latest
```

> *Omit `<REGISTRY>/` prefix if using local images.*

## Verification & Cleanup

- **Status**: `docker-compose ps`
- **Logs**: `docker-compose logs -f api`, `docker-compose logs -f frontend`
- **UI**: http://localhost
- **API**: http://localhost:8080

Remove containers and networks:

```bash
docker-compose down --remove-orphans
```

What you should see if the front-end is healthy:
![main front-end](./docImage/photo_2025-06-12_22-15-31.jpg?raw=true)

For api service we should have something like this or an empty json as well:
![api response](./docImage/photo_2025-06-12_22-15-44.jpg?raw=true)


## License

This project is licensed under the Apache License 2.0.  
See the [LICENSE](../LICENSE) file for details.
