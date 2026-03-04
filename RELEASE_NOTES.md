# Release Notes — v1.0.0

**Release Date:** 2026-03-04

## Highlights

Initial release of **Bookjobs**, a containerized Python REST API built with Flask and deployed via Azure Pipelines to Azure Kubernetes Service (AKS). This release establishes the full CI/CD pipeline, Docker containerization, and Kubernetes manifests for the application.

## New Features

- **Bookjobs REST API** — Flask-based API with three endpoints:
  - `POST /bookjobs` — Create a new job
  - `GET /bookjobs` / `GET /bookjobs/{id}` — List all jobs or retrieve a specific job by ID
  - `DELETE /bookjobs/{id}` — Delete a job by ID
- **Docker containerization** — Dockerfile for building and running the Flask app as a container; supports port binding (`docker run -p 80:5000`)
- **Kubernetes manifests** — Deployment and Service YAML manifests for running the app on AKS
- **Azure Pipelines CI/CD** — Multiple pipeline configurations (`azure-pipelines.yml`, `-1`, `-3`, `-4`, `-5`) covering build, push to container registry, and deploy to AKS
- **Postman collection** — Pre-built request collection for testing the REST API endpoints

## Improvements

- Iterative Azure Pipelines configuration hardening across multiple pipeline files
- Added separate pipeline variants for different deployment stages and testing scenarios
- Introduced `.dockerignore` and `.gitignore` for clean builds and repository hygiene

## Other Changes

- Added `resources.txt` documenting Azure resource references
- Added `requirements.txt` for Python dependency tracking (`flask`)
- Added `curl_commands.sh` for quick CLI-based API testing against the containerized app
