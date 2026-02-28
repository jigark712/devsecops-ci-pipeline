![ci](https://github.com/jigark712/devsecops-ci-pipeline/actions/workflows/ci.yml/badge.svg)
![codeql](https://github.com/jigark712/devsecops-ci-pipeline/actions/workflows/codeql.yml/badge.svg)
![sbom](https://github.com/jigark712/devsecops-ci-pipeline/actions/workflows/sbom.yml/badge.svg)

# DevSecOps CI/CD Pipeline (Python + Docker)

This repository demonstrates a realistic DevSecOps workflow around a small Dockerized Python service. It automates testing and security checks on every push and pull request and publishes outputs as downloadable artifacts.

The goal is to show an end-to-end pipeline that a team can use to keep builds reproducible, detect vulnerabilities early, and keep a traceable inventory of shipped components.

## What is implemented

### Application
- Minimal Flask service with a health endpoint: `GET /health`
- Unit test coverage for the endpoint

### CI/CD (GitHub Actions)
- Runs tests on every push and pull request
- Builds a Docker image in CI for reproducible packaging

### Security automation
- Static analysis (CodeQL) for code-level security issues
- Dependency auditing (pip-audit) for Python dependency vulnerabilities
- Container vulnerability scanning (Trivy) on the built Docker image (SARIF output)
- SBOM generation (Syft) for supply-chain traceability (SPDX JSON)

### Outputs
- CI uploads scan outputs as artifacts:
  - `trivy-results.sarif`
  - `sbom.spdx.json`

## Repository structure
- `app/` — service code (`main.py`)
- `tests/` — unit tests
- `Dockerfile` — container build
- `requirements.txt` — pinned Python dependencies
- `pytest.ini` — pytest config to keep imports consistent
- `.github/workflows/` — CI, CodeQL, SBOM workflows

## Run locally (Python)

Prereqs:
- Python 3.10+ (CI uses 3.11)
- macOS/Linux shell

Steps:
1) Create and activate a virtual environment
   - `python3 -m venv .venv`
   - `source .venv/bin/activate`

2) Install dependencies
   - `pip install -U pip`
   - `pip install -r requirements.txt`

3) Run tests
   - `pytest -q`

4) Run the service
   - `python app/main.py`

5) Verify
   - `curl http://localhost:8080/health`

## Run locally (Docker)

Prereqs:
- Docker Desktop running

Steps:
1) Build the image
   - `docker build -t devsecops-ci-pipeline:local .`

2) Run the container
   - `docker run --rm -p 8080:8080 devsecops-ci-pipeline:local`

3) Verify
   - `curl http://localhost:8080/health`


