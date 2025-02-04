# K8s-MongoDB-Express-Local

## Overview
In this project, I deployed MongoDB and Mongo Express into a local Kubernetes cluster, providing a complete local database solution with web-based management interface.

## Technologies Used
- Kubernetes
- Docker
- MongoDB
- Mongo Express
- Minikube

## Architecture
The deployment consists of:
- MongoDB pod with persistent storage
- Mongo Express pod with web interface
- ConfigMap for database configurations
- Secret for storing sensitive credentials
- Services for both MongoDB and Mongo Express

![Diagram](https://github.com/Princeton45/k8s-mongodb-express-local/blob/main/images/diagram.png)

## Implementation Details

### Local Environment Setup
Created a local Kubernetes cluster using Minikube on my machine.

![Minikube Startup](images/minikube-startup.png)

### MongoDB Deployment
Deployed MongoDB with secure configurations using ConfigMaps and Secrets for credential management.

![MongoDB Pods](images/mongodb-pods.png)

### Mongo Express Interface
Successfully connected Mongo Express to the MongoDB instance, providing web-based database management.

![Mongo Express](images/mongo-express.png)

## Architecture
The deployment consists of:
- MongoDB pod with persistent storage
- Mongo Express pod with web interface
- ConfigMap for database configurations
- Secret for storing sensitive credentials
- Services for both MongoDB and Mongo Express

