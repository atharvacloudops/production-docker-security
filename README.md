# Production Docker & Container Security

A complete containerisation project covering multi-stage builds, security scanning, 
multi-service orchestration, and container runtime security.

## Project Structure
production-docker-security/
├── app/
│   ├── main.py              # FastAPI application
│   └── requirements.txt     # Python dependencies
├── .github/
│   └── workflows/
│       └── trivy.yml        # CI security scanning pipeline
├── Dockerfile.naive         # Baseline image ~500MB
├── Dockerfile.optimised     # Optimised slim image ~180MB
├── Dockerfile.final         # Multi-stage alpine image <100MB
├── Dockerfile.buildkit      # BuildKit with cache mounts
├── docker-compose.yml       # Multi-service orchestration
├── docker-compose.dev.yml   # Development overrides
├── docker-compose.prod.yml  # Production overrides
├── seccomp-profile.json     # Container syscall restrictions
└── .dockerignore            # Build context exclusions

---

## 📊 Image Size Optimisation

| Dockerfile | Base Image | Size | Technique |
|---|---|---|---|
| Dockerfile.naive | python:3.11 | ~500MB | No optimisation |
| Dockerfile.optimised | python:3.11-slim | ~180MB | Layer ordering + no cache |
| Dockerfile.final | python:3.11-alpine | <100MB | Multi-stage + alpine |
| Dockerfile.buildkit | python:3.11-alpine | <100MB | BuildKit cache mounts |

> 85% image size reduction from naive to final 🚀

---

## ✨ Key Features

### 🏗️ Multi-Stage Builds
Reduced Docker image size by 85% using builder and runtime stages.
Only production dependencies are copied to the final image.

### 🔒 Security
- ✅ Non-root user inside container
- ✅ Seccomp profile to restrict allowed syscalls
- ✅ `.dockerignore` to prevent sensitive files entering build context
- ✅ Trivy vulnerability scanning on every push via GitHub Actions

### ⚡ BuildKit Advanced Features
- ✅ Cache mounts for faster pip installs across builds
- ✅ Multi-architecture build supporting `linux/amd64` and `linux/arm64`

### 🐙 Docker Compose Orchestration
Multi-service setup including:
- ✅ FastAPI app
- ✅ PostgreSQL with health checks
- ✅ Redis
- ✅ Nginx reverse proxy
- ✅ Dev and prod override files

### 🔄 CI/CD Pipeline
GitHub Actions workflow runs Trivy image scan on every push.
Pipeline blocks deployment if CRITICAL vulnerabilities are found.

---

## How to Run

### Single Container
```bash
docker build -f Dockerfile.final -t myapp:final .
docker run -p 8000:8000 myapp:final
```

Visit `http://localhost:8000` to see the app running.

### Full Stack
```bash
docker compose up
```

### Multi-Arch Build
```bash
docker buildx create --name multiarch --use
docker buildx build --platform linux/amd64,linux/arm64 -f Dockerfile.final -t myapp:multiarch --load .
```

## Tech Stack
Docker · FastAPI · PostgreSQL · Redis · Nginx · GitHub Actions · Trivy · BuildKit
