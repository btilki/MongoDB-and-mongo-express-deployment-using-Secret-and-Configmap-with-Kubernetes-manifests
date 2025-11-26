# MongoDB and mongo-express Deployment using Secret and ConfigMap with Kubernetes Manifests

This repository provides ready-to-use Kubernetes manifests for deploying **MongoDB** and **mongo-express** on any Kubernetes cluster. It demonstrates how to utilize **Secrets** and **ConfigMaps** for secure and dynamic configuration management, following best practices for running databases and web interfaces in a cloud-native manner.

## Features

- **MongoDB deployment:** Containerized NoSQL database.
- **mongo-express deployment:** Web-based UI to interact with MongoDB.
- **Kubernetes Secrets:** Securely store MongoDB credentials.
- **Kubernetes ConfigMaps:** Inject configuration variables.
- **YAML manifests:** Modular, easy to customize, production-ready.

## Architecture

```
+----------------+        +--------------------+
|                |        |                    |
|    MongoDB     | <----> |    mongo-express   |
|                |        |                    |
+----------------+        +--------------------+
      ^                          ^
      |                          |
   Secrets &                 ConfigMaps &
  Persistent Storage        Environment Variables
```

- MongoDB runs in its own pod/statefulset and is configured via secrets and configmaps.
- mongo-express is deployed as a web pod, utilizing the same config/secrets to connect to the DB.

## Prerequisites

- Access to a Kubernetes cluster (local or cloud)
- `kubectl` installed and configured

## Start Minikube using Docker driver
```bash
minikube start --driver=docker
```
## Setup & Usage

### 1. Clone the Repository

```bash
git clone https://github.com/btilki/MongoDB-and-mongo-express-deployment-using-Secret-and-Configmap-with-Kubernetes-manifests.git
```
```bash
cd MongoDB-and-mongo-express-deployment-using-Secret-and-Configmap-with-Kubernetes-manifests
```

### 2. Create Secrets
Use base64 encoding for username and password in the secret.yaml file.
Sample usage: 
```bash
echo -n 'username' | base64
```
Create Kubernetes secrets for MongoDB credentials:

```bash
kubectl apply -f mongo-secret.yaml
```
Note: The secret must be created before deployment.
### 3. Create ConfigMaps

Create configmaps for environment variables (e.g., database name):

```bash
kubectl apply -f mongo-configmap.yaml
```
Note: Also ConfigMap must be created before deployment.
### 4. Deploy MongoDB

```bash
kubectl apply -f mongo.yaml
```

### 5. Deploy mongo-express

```bash
kubectl apply -f mongo-express.yaml
```

Use NodePort or LoadBalancer service to access Mongo-express.
To get service information:
```bash
kubectl get service
```
To start the Mongo-Express service, when minikube is used:
```bash
minikube service mongo-express-service
```
## Troubleshooting

- Check pod status:  
  `kubectl get pods`
- Examine logs:  
  `kubectl logs <pod-name>`
- Verify secrets/configmaps are mounted as expected.

## License

MIT
