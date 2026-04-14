# Production Docker & Container Security

## Day 1 - Multi-Stage Dockerfile Mastery

### Image Size Comparison
| Dockerfile | Base Image | Size |
|---|---|---|
| Dockerfile.naive | python:3.11 | ~500MB |
| Dockerfile.optimised | python:3.11-slim | ~180MB |
| Dockerfile.final | python:3.11-alpine (multi-stage) | ~85MB |

### Key Techniques Used
- Multi-stage builds (builder + runtime stages)
- Alpine base image
- Layer cache ordering
- .dockerignore to exclude unnecessary files
- Non-root user for security

### How to Run
docker build -f Dockerfile.final -t myapp:final .
docker run -p 8000:8000 myapp:final