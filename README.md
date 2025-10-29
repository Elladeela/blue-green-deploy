# Blue-Green Deployment with Nginx + Docker Compose (Failover Ready)

This project implements a *Blue/Green deployment strategy* using *Docker Compose and Nginx* to achieve *zero-downtime failover* between two identical Node.js services. Blue is the active version by default, while Green serves as the standby release. Nginx is configured to automatically route traffic to Green if Blue becomes unhealthy.

---

## ✅ Architecture Overview

- *Nginx* load balancer (failover + retry rules)
- *Blue App* – Primary instance
- *Green App* – Standby instance
- *Failover* triggered automatically on errors/timeouts
- *Chaos testing support* for reliability validation
- *Docker Compose* for orchestration
- *Environment-based configuration* via .env

---

## ✅ Endpoints

| Service | Endpoint | Purpose |
|----------|-----------|----------|
| Blue App | http://localhost:8081 | Direct access for chaos testing |
| Green App | http://localhost:8082 | Backup instance |
| Nginx Frontend | http://localhost:8080 | Public entrypoint |
| /version | Returns app version & headers |
| /healthz | Health check |
| /chaos/start | Simulates service failure |
| /chaos/stop | Restores service |

---

## ✅ Requirements

- Docker + Docker Compose installed
- .env file configured
- Internet connection

---

## ✅ Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/Elladeela/blue-green-deploy.git
cd blue-green-deploy
2. Configure Environment
Create .env file:

bash
Copy code
cp .env .env.example.
Edit .env.example and set
env
Copy code
BLUE_IMAGE=yimikaade/wonderful:devops-stage-two
GREEN_IMAGE=yimikaade/wonderful:devops-stage-two
ACTIVE_POOL=blue
RELEASE_ID_BLUE=v1-blue
RELEASE_ID_GREEN=v1-green
PORT=3000
3. Start the Services
bash
Copy code
docker-compose up -d
✅ Test Deployment
✅ Check active version:

bash
Copy code
curl -i http://localhost:8080/version
Should show:

makefile
Copy code
X-App-Pool: blue
🔥 Trigger failover:

bash
Copy code
curl -X POST "http://localhost:8081/chaos/start?mode=error"
✅ Test failover:

bash
Copy code
curl -i http://localhost:8080/version
Should now show:

makefile
Copy code
X-App-Pool: green
🔁 Restore Blue:

bash
Copy code
curl -X POST "http://localhost:8081/chaos/stop"
✅ Stop Services
bash
Copy code
docker-compose down
✅ Deployment URL (Live)
arduino
Copy code
http://18.234.87.172:8080/version
✅ Technologies Used
Docker & Docker Compose

Nginx

Node.js services (pre-built images)

Chaos Engineering (failover testing)