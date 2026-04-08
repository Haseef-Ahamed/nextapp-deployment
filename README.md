# NextStep — Production ML Deployment on AWS EC2

> DevOps + MLOps deployment of a 3-tier AI-powered career guidance application for Sri Lankan students.

## What I Deployed

| Service | Technology | Port |
|---------|-----------|------|
| Frontend | React + Nginx (Docker) | 80 |
| Backend | Express.js + MySQL (Docker) | 5000 |
| ML Service | Flask + Gunicorn + XGBoost (Docker) | 5002 |
| Database | MySQL 8.0 (Docker) | 3306 |

## Architecture
## My DevOps Work

- Configured AWS EC2 (Ubuntu 24.04, t3.xlarge, Elastic IP)
- Wrote all 4 Dockerfiles (Backend, Frontend multi-stage, 2x ML)
- Configured Nginx as reverse proxy — eliminating CORS issues
- Wrote docker-compose.yml with health checks and dependency ordering
- Configured 4GB swap memory for ML model loading on 4GB RAM
- Set up MySQL with auto-initialization via init-database.sql mount
- Fixed 14 deployment issues (see Issues & Fixes below)

## ML Models Deployed

- **Course Prediction** — XGBoost, 330 Sri Lankan university course+university combinations
- **Job Prediction** — XGBoost, 12 job types

Both models verified working with curl tests:

```bash
# Job prediction
curl -X POST http://3.21.95.249:5002/job/predict \
  -H "Content-Type: application/json" \
  -d '{"age":25,"gender":"Male","degree":"BSc Hons","field_of_study":"IT",
       "work_experience_years":2,"certifications":"Yes","skills_count":5,
       "preferred_location":"Colombo"}'

# Response: Software Engineer — 62.43% confidence
```

## Key Issues Solved

| Problem | Fix |
|---------|-----|
| PEM file bad permissions on Windows | icacls /inheritance:r |
| SCP path canonicalization failed | Added -O flag |
| ML models nested 3 levels deep (Mac ZIP) | mv + rm -rf |
| Port 80 blocked by system Nginx | systemctl stop + disable nginx |
| flask_cors not found (Docker cache) | docker rmi --force + --no-cache rebuild |
| React routes return 404 | try_files $uri $uri/ /index.html |
| Browser timeout on ML port 5002 | Nginx proxy /ml/ → ml:5002 |
| MySQL case sensitivity (NEXTSTEP vs nextstep) | Matched uppercase everywhere |

## Tech Stack

`AWS EC2` `Docker 29.3.1` `Docker Compose v5.1.1` `Nginx` `Node.js 18` `Python 3.11` `Flask` `Gunicorn` `XGBoost` `MySQL 8.0` `React` `TypeScript`

## Deployment Status
---
*Note: Application source code is proprietary and not included in this repository.
This repo contains only the DevOps/MLOps configuration files I created.*
