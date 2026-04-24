# EmartApp Main - Containerized Microservices E-Commerce Project

This repository contains a containerized e-commerce application built with multiple services:

- Angular frontend (`client`)
- Node.js + Express API (`nodeapi`)
- Spring Boot API (`javaapi`)
- NGINX reverse proxy (`nginx`)
- MongoDB and MySQL databases
- Helm chart for Kubernetes deployment (`kkartchart`)
- Jenkins pipeline files for CI/CD

## Project Architecture

Main service flow:

1. User opens frontend on port `4200`.
2. Frontend calls backend APIs:
- Node API on port `5000`
- Java API on port `9000`
3. Node API connects to MongoDB (`emongo`).
4. Java API connects to MySQL (`emartdb`).
5. NGINX (port `80`) is used as gateway/reverse proxy in container setup.

## Tech Stack

- Frontend: Angular 12, Bootstrap
- Node Backend: Node.js, Express, Mongoose
- Java Backend: Spring Boot 2.3.1, Spring Data JPA
- Databases: MongoDB 4, MySQL 8.0.33
- Containerization: Docker, Docker Compose
- Orchestration (optional): Helm/Kubernetes
- CI/CD: Jenkins

## Repository Structure

```text
.
|-- client/           # Angular frontend app
|-- nodeapi/          # Node.js backend API (MongoDB)
|-- javaapi/          # Spring Boot backend API (MySQL)
|-- nginx/            # NGINX reverse proxy configs
|-- kkartchart/       # Helm chart for Kubernetes
|-- docker-compose.yaml
|-- Dockerfile        # Combined image build (UI + Node API)
|-- Jenkinsfile
```

## Exposed Ports

- `4200` -> Angular frontend
- `5000` -> Node API
- `9000` -> Java API
- `80` -> NGINX gateway
- `27017` -> MongoDB
- `3306` -> MySQL

## Prerequisites

Install the following tools:

- Docker Desktop (recommended for easiest run)
- Docker Compose
- Node.js 14+ (for local non-container run)
- Java 8 and Maven (for `javaapi` local run)

## Quick Start (Docker Compose)

From repository root:

```bash
docker compose up --build
```

Run in detached mode:

```bash
docker compose up --build -d
```

Stop services:

```bash
docker compose down
```

After startup, access app at:

- Frontend: `http://localhost:4200`
- NGINX gateway: `http://localhost:80`

## Local Development Setup (Without Compose)

### 1) Start Databases

Start MongoDB and MySQL locally (or using containers) with these defaults:

- MongoDB: `mongodb://localhost:27017/epoc`
- MySQL DB: `books`
- MySQL user: `root`
- MySQL password: `emartdbpass`

### 2) Run Node API

```bash
cd nodeapi
npm install
npm run server
```

Node API starts on `http://localhost:5000`.

### 3) Run Java API

```bash
cd javaapi
./mvnw spring-boot:run
```

Java API starts on `http://localhost:9000`.

### 4) Run Angular Frontend

```bash
cd client
npm install
npm start
```

Frontend starts on `http://localhost:4200`.

## Service Configuration Notes

- Java API database config is in `javaapi/src/main/resources/application.properties`.
- Frontend backend ports are configured in `client/src/app/backend_config/backend-config.service.ts`.
- Node API Mongo connection string is configured in `nodeapi/config/keys.js`.

## Docker Files in This Repo

- Root `Dockerfile`: multi-stage build for Angular UI + Node API.
- `client/Dockerfile`: builds Angular app and serves via NGINX.
- `javaapi/Dockerfile`: Java backend container image.
- `nodeapi/Dockerfile`: Node backend container image.

## Kubernetes (Helm)

Helm chart is available in `kkartchart/`.

Typical workflow:

```bash
helm lint kkartchart
helm install emart kkartchart
```

## CI/CD

Jenkins files are present at:

- Root: `Jenkinsfile`
- `client/Jenkinsfile`
- `javaapi/Jenkinsfile`

Use these as pipeline templates for service-wise or full-stack builds.

## Troubleshooting

- Port conflict: change host ports in `docker-compose.yaml`.
- Database connection issue: verify DB containers are healthy and credentials match config files.
- Angular build issue: remove `node_modules` and reinstall packages in `client`.
- Java startup failure: ensure Java 8 is available (`java -version`).

## License

Use according to your organization or learning requirements.
