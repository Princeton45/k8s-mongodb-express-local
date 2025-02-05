# Deploying MongoDB and Mongo Express into local Kubernetes (Minikube) Cluster

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

![minikube](https://github.com/Princeton45/k8s-mongodb-express-local/blob/main/images/minikube.png)

### MongoDB Deployment
Deployed MongoDB with secure configurations using ConfigMaps and Secrets for credential management.

`mongodb-deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USERNAME
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-username
        - name: MONGO_INITDB_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-password
---
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
  - protocol: TCP
    port: 27017
    targetPort: 27017
```

`db-secret.yml`
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mongodb-secret
type: Opaque
data:
  mongo-root-username: dXNlcm5hbWU=
  mongo-root-password: cGFzc3dvcmQ=
```

`configmap.yml`
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  database_url: mongodb-service
```

![resources](https://github.com/Princeton45/k8s-mongodb-express-local/blob/main/images/apps.png)

### Mongo Express Interface
Successfully connected Mongo Express to the MongoDB instance, providing web-based database management.

`mongo-express.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-express
  labels:
    app: mongo-express
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo-express
  template:
    metadata:
      labels:
        app: mongo-express
    spec:
      containers:
      - name: mongo-express
        image: mongo-express
        ports:
        - containerPort: 8081
        env:
        - name: ME_CONFIG_MONGODB_ADMINUSERNAME
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-username
        - name: ME_CONFIG_MONGODB_ADMINPASSWORD
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-password
        - name: ME_CONFIG_MONGODB_SERVER
          valueFrom:
            configMapKeyRef:
              name: mongodb-configmap
              key: database_url
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-express-service
spec:
  selector:
    app: mongo-express
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 8081
    targetPort: 8081
    nodePort: 30000
```

![express](https://github.com/Princeton45/k8s-mongodb-express-local/blob/main/images/express.png)


