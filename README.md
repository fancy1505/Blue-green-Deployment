# Blue-Green Deployment Application using Docker and Kubernetes

## Project Overview

This project demonstrates the deployment of a Node.js application using Docker and Kubernetes (Minikube) with a Blue-Green deployment strategy.

The application consists of:

* Backend API (Node.js + Express)
* MongoDB Database
* Frontend Blue (Basic Version)
* Frontend Green (Enhanced Version)

The objective was to containerize the application, deploy it on Kubernetes, and implement Blue-Green deployment for zero-downtime releases.

---

# Part 1: Local Deployment

## Steps Performed

1. Cloned the Git repository.
2. Installed dependencies for:

   * Backend
   * Frontend Blue
   * Frontend Green
3. Configured MongoDB connection using environment variables.
4. Started MongoDB locally.
5. Started backend and frontend services.
# <img width="1011" height="878" alt="image" src="https://github.com/user-attachments/assets/024370d6-a66f-4bb3-81cc-202f0156b4fd" />
# <img width="1045" height="586" alt="image" src="https://github.com/user-attachments/assets/19c85c1c-da15-499f-83fc-1729060f3c60" />
# <img width="1056" height="445" alt="image" src="https://github.com/user-attachments/assets/5627373f-67ae-4208-b06e-6304576d3d9f" />
# <img width="1072" height="450" alt="image" src="https://github.com/user-attachments/assets/fac5ed1e-7989-4d66-8faf-acc6b672d125" />


## Verification

* Backend health endpoint responded successfully.
* Frontend Blue was accessible on port 3100.
* Frontend Green was accessible on port 3200.
* User registration data was successfully stored in MongoDB.
 # <img width="1033" height="252" alt="image" src="https://github.com/user-attachments/assets/18f60365-22a4-483a-883a-d0d10f4da6c4" />
# <img width="1068" height="680" alt="image" src="https://github.com/user-attachments/assets/f05d732b-edd8-4d57-adbb-f879e2f93d12" />
# <img width="1072" height="769" alt="image" src="https://github.com/user-attachments/assets/f740af7d-0a1d-4f6f-b807-968516140e04" />



---

# Part 2: Containerization
# <img width="994" height="534" alt="image" src="https://github.com/user-attachments/assets/aff6c9f5-ae2e-43e5-8f04-2d9452c51633" />


## Dockerfiles Created

Dockerfiles were created for:

* Backend
* Frontend Blue
* Frontend Green
* # <img width="1040" height="1259" alt="image" src="https://github.com/user-attachments/assets/800f2291-2afb-405b-82e8-9cd5f0973942" />


## Docker Compose

A docker-compose.yml file was created to manage:

* MongoDB
* Backend
* Frontend Blue
* Frontend Green

## Commands Used

```bash
docker compose up --build
docker ps
docker images
```

## Verification

* Docker images built successfully.
* Containers started successfully.
* Application components communicated correctly through Docker networking.

---

# Part 3: Kubernetes Deployment

## Kubernetes Resources Created

### Deployments

* mongodb
* backend
* frontend-blue
* frontend-green
# <img width="943" height="817" alt="image" src="https://github.com/user-attachments/assets/91e4a948-3bcc-4ef3-8b40-6c3dfa214f87" />
# <img width="944" height="505" alt="image" src="https://github.com/user-attachments/assets/e07afec4-2352-448a-9bf4-0ab7a2af076b" />
# <img width="1033" height="253" alt="image" src="https://github.com/user-attachments/assets/06b870be-f55c-4590-8f94-bbb970fdc614" />


### Services

* mongodb
* backend
* frontend-service
* # <img width="763" height="513" alt="image" src="https://github.com/user-attachments/assets/ee382ac0-b79c-4c92-810a-d7f77aacae1e" />


## Commands Used

```bash
minikube start

kubectl apply -f k8s/

kubectl get deployments
kubectl get pods
kubectl get svc
```

## Verification

All pods reached Running state:

* MongoDB
* Backend
* Frontend Blue
* Frontend Green

Services were successfully exposed and reachable.

---# <img width="772" height="538" alt="image" src="https://github.com/user-attachments/assets/9c78fe74-2301-44c3-ae59-348c371d871c" />


# Part 4: Blue-Green Deployment Implementation

## Strategy

Two frontend deployments were maintained simultaneously:

### Blue Deployment

Frontend Blue represents the currently active production version.

### Green Deployment

Frontend Green represents the new version ready for release.

A Kubernetes Service named frontend-service acts as a traffic router.

Initially traffic is routed to:

```yaml
selector:
  app: frontend
  version: blue
```

To switch traffic:

```yaml
selector:
  app: frontend
  version: green
```

Apply the change:

```bash
kubectl apply -f k8s/frontend-service.yaml
```

This redirects all traffic to the Green deployment without downtime.

Rollback can be performed instantly by switching the selector back to Blue.
# <img width="2487" height="1645" alt="image" src="https://github.com/user-attachments/assets/e1eb3ff9-8c6e-4fe5-902b-e1f9517b7d04" />
# <img width="3300" height="2412" alt="image" src="https://github.com/user-attachments/assets/3db99dde-bb5d-4b0a-90d1-b574c06df146" />
---

# Challenges Faced

## MongoDB Connection Issue

Problem:

Backend failed to connect because MONGO_URI was not configured.

Solution:

Created .env file and configured:

```env
MONGO_URI=mongodb://localhost:27017/usersdb
```

---

## Docker Port Conflict

Problem:

MongoDB port 27017 was already in use.

Solution:

Removed old containers and recreated the environment using Docker Compose.

---

## Kubernetes Image Availability

Problem:

Locally built images were not available inside Minikube.

Solution:

Loaded images into Minikube using:

```bash
minikube image load blue-green-deployment-backend:latest

minikube image load blue-green-deployment-frontend-blue:latest

minikube image load blue-green-deployment-frontend-green:latest
```

---

# Conclusion

Successfully completed:

* Local deployment
* Docker containerization
* Docker Compose orchestration
* Kubernetes deployment on Minikube
* Blue-Green deployment implementation
* Traffic switching between Blue and Green versions

