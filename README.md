🍽️ Microservices App Deployment
================================

This repository contains **three Node.js microservices** (`auth`, `discounts`, and `items`) and a **React frontend** (`client`), all orchestrated via Kubernetes for a unified deployment experience.

Source code for the Node.js microservices and React frontend originally comes from: [https://github.com/Pokfinner/restauranty](https://github.com/Pokfinner/restauranty)

🔁 Unified Routing
------------------

In production, all services are accessed under a single domain/IP using URL path prefixes:

*   `/api/auth/...` → **Auth Service**
*   `/api/discounts/...` → **Discounts Service**
*   `/api/items/...` → **Items Service**
*   `/` → **React Frontend**

📷 Architecture Overview
------------------------

![image](https://github.com/user-attachments/assets/afd87f2a-d7bd-4218-90be-4825a32a610c)


🧪 Local Development (Using HAProxy)
------------------------------------

1.  Start each service from its directory using `npm install` followed by `npm start`.
2.  Ensure that each service is running on its assigned port (e.g., 3001, 3002, etc.).
3.  Launch HAProxy from the root directory using:  
    `haproxy -f haproxy.cfg`
4.  Access the full app at [http://localhost](http://localhost). HAProxy will route traffic to the correct microservice based on the path prefix.

### 🌍 Frontend Environment Variables

*   `REACT_APP_API_URL=http://localhost:80` (for HAProxy routing)
*   `REACT_APP_SERVER_URL=/api` (for Docker/EKS deployments)

🐳 Docker Containerization
--------------------------

Each service and the frontend is containerized with Docker.

1.  Ensure each service and the frontend have a valid `Dockerfile`.
2.  Build images using:  
    `docker build -t your-dockerhub-username/service-name .`
3.  Tag and push each image to your Docker registry:  
    `docker push your-dockerhub-username/service-name`

☸️ Kubernetes Deployment (EKS)
------------------------------

All microservices are deployed to an AWS EKS cluster using Kubernetes manifests. Includes:

*   Deployment files for each service
*   Services and Ingress definitions
*   MongoDB managed via Helm

🌐 Ingress Controller
---------------------

Use **NGINX Ingress Controller** to expose the services behind a single domain. It handles routing based on the URL path prefixes as shown above.

🔄 CI/CD Pipeline (GitHub Actions)
----------------------------------

Located in `.github/workflows`, the pipeline includes:

*   Installing and building each microservice and the React client
*   Building Docker images and pushing them to Docker Hub
*   Applying updated Kubernetes manifests
*   Triggering Helm release for MongoDB database

🔐 Secret Management
--------------------

All sensitive values and credentials are stored securely using **GitHub Secrets** and accessed during CI/CD execution.

* * *

🎯 This setup demonstrates a production-grade microservices deployment pipeline with scalable architecture and automated delivery using Kubernetes and GitHub Actions.
