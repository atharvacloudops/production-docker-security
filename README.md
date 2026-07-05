# Production Docker & Container Security

3-week internship project covering containerisation, security scanning, 
multi-service orchestration and runtime security.

## Project Structure
- Dockerfile.naive — baseline ~500MB image
- Dockerfile.optimised — slim image ~180MB  
- Dockerfile.final — multi-stage alpine <100MB
- docker-compose.yml — 4 service orchestration
- .github/workflows — Trivy CI scanning

## How to Run

### Single container
docker build -f Dockerfile.final -t myapp:final .
docker run -p 8000:8000 myapp:final

### Full stack
docker compose up

## Image Size Comparison
| Dockerfile | Base | Size |
|---|---|---|
| naive | python:3.11 | ~500MB |
| optimised | python:3.11-slim | ~180MB |
| final | python:3.11-alpine multi-stage | <100MB |

## Tech Stack
Docker, FastAPI, PostgreSQL, Redis, Nginx, GitHub Actions, Trivy 