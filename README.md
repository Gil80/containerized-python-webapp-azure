# Containerized Python Web App — Azure

A containerized Flask REST API deployed to **Azure Kubernetes Service (AKS)** via **Azure Pipelines** CI/CD. The application manages "book jobs" and demonstrates a complete cloud-native Python deployment workflow on Azure.

## Architecture

```
Flask App (Python)
    └── Docker Container (python:alpine)
            └── Azure Container Registry (ACR)
                    └── Azure Kubernetes Service (AKS)
                            └── Kubernetes Service (LoadBalancer)
```

CI/CD is handled by Azure Pipelines, which builds the Docker image, pushes it to ACR, and deploys it to AKS on every push to `main`.

## Application

**Bookjobs API** — a simple CRUD REST API built with Flask.

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/bookjobs` | List all book jobs |
| `GET` | `/bookjobs/{id}` | Get a specific book job by ID |
| `POST` | `/bookjobs` | Create a new book job |
| `DELETE` | `/bookjobs/{id}` | Delete a book job by ID |

Data is persisted to `data.json` on disk.

## Project Structure

```
.
├── app/
│   ├── app.py              # Flask application
│   ├── Dockerfile          # Container image definition
│   ├── requirements.txt    # Python dependencies (flask)
│   ├── data.json           # Persistent data store
│   └── curl_commands.sh    # CLI test commands
├── manifests/
│   ├── deployment.yml      # Kubernetes Deployment
│   └── service.yml         # Kubernetes Service (LoadBalancer)
├── azure-pipelines.yml     # CI pipeline (test, Python 3.9)
├── azure-pipelines-3.yml   # CD pipeline (build → push ACR → deploy AKS)
├── azure-pipelines-4.yml   # Alternate pipeline configuration
├── azure-pipelines-5.yml   # Alternate pipeline configuration
├── azure-rest/             # Postman collection for API testing
├── INSTRUCTIONS.md         # Setup and usage instructions
└── RELEASE_NOTES.md        # Version history
```

## Prerequisites

- Docker
- Azure CLI
- An Azure subscription with:
  - Azure Container Registry (ACR)
  - Azure Kubernetes Service (AKS)
  - Azure DevOps project with a service connection to ACR and AKS

## Running Locally

```bash
cd app
pip install -r requirements.txt
flask --app app run
```

The API will be available at `http://127.0.0.1:5000`.

## Running with Docker

```bash
cd app
docker build -t bookjobs .
docker run -p 80:5000 bookjobs
```

The API will be available at `http://localhost/bookjobs`.

## CI/CD Pipeline

The main pipeline (`azure-pipelines-3.yml`) runs on push to `main` and:

1. **Build** — builds the Docker image and pushes it to ACR tagged with the build ID
2. **Deploy** — applies the Kubernetes manifests to AKS and pulls the new image from ACR
3. **PR Preview** — deploys pull requests to an isolated namespace for review

## Testing

Use the included Postman collection (`azure-rest/`) or the `curl_commands.sh` script:

```bash
cd app
bash curl_commands.sh
```

See [INSTRUCTIONS.md](INSTRUCTIONS.md) for detailed usage steps.
