# 12 — Deployment (Phase 11–12: Docker & AWS)

## Local Development (Docker Compose)

`docker/docker-compose.yml` should spin up:
```
- postgres        (port 5432)
- redis           (port 6379)
- kafka + zookeeper (or KRaft mode, no zookeeper)
- backend         (Spring Boot, built from backend/Dockerfile)
- frontend        (React, built from frontend/Dockerfile)
```

One command: `docker compose up` → full stack running locally.

## Backend Dockerfile (multi-stage, outline)
```
Stage 1 (build): use Maven image → mvn package → produces jar
Stage 2 (run):   use slim JRE image → copy jar → ENTRYPOINT java -jar app.jar
```
Multi-stage keeps final image small (no build tools in production image).

## Frontend Dockerfile (outline)
```
Stage 1 (build): node image → npm install → npm run build → static files
Stage 2 (serve): nginx image → copy static build → serve
```

## AWS Deployment (Phase 12)

```
Route53 (DNS) → ALB (Application Load Balancer)
                     │
                     ▼
        ECS (Fargate) or EC2 running backend container
                     │
        ┌────────────┼─────────────┐
        ▼            ▼             ▼
   RDS (Postgres) ElastiCache   MSK (managed Kafka)
                    (Redis)     or self-hosted Kafka on EC2
```

- **S3**: static frontend hosting (or serve via CloudFront + S3) OR bundle
  into same ECS task if keeping it simple for V1.
- **Secrets**: AWS Secrets Manager or Parameter Store for DB
  password/JWT secret — never in `application.yml` committed to Git.
- **CI/CD**: GitHub Actions (`.github/workflows/`) — build → test → build
  Docker image → push to ECR → deploy to ECS on merge to `main`.

## Environments
| Env | Purpose | Config file |
|---|---|---|
| dev | Local docker-compose | `application-dev.yml` |
| prod | AWS | `application-prod.yml` (values injected via env vars/Secrets Manager) |

## Checklist
- [ ] `backend/Dockerfile` (multi-stage)
- [ ] `frontend/Dockerfile` (multi-stage)
- [ ] `docker/docker-compose.yml` for local dev
- [ ] `.github/workflows/backend-ci.yml` — build + test on PR
- [ ] `.github/workflows/frontend-ci.yml`
- [ ] AWS: RDS Postgres instance provisioned
- [ ] AWS: ElastiCache Redis provisioned
- [ ] AWS: Kafka (MSK or self-hosted) provisioned
- [ ] Secrets stored in Secrets Manager, not in code
- [ ] Health check endpoint (`/actuator/health`) wired to ALB target group

## Done Criteria
`docker compose up` runs the entire stack locally end-to-end; a merge to
`main` auto-deploys backend to AWS via GitHub Actions.
