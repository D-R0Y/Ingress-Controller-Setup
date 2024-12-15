# Kubernetes Deployment for Frontend and Backend Applications

This repository contains Kubernetes deployment configurations for a sample frontend React application and backend Node.js API. The setup includes deployment, service, and ingress configurations for a scalable and domain-routed application architecture.

## Deployment Configurations

### 1. Frontend Deployment
- **Image**: `frontend-react-app:latest`
- **Replicas**: 2 (for high availability)
- **Resources**:
  - **Requests**: Defined to ensure guaranteed resources
  - **Limits**: Defined to cap resource usage
- **Exposed Port**: 80

### 2. Backend Deployment
- **Image**: `backend-nodejs-api:latest`
- **Replicas**: 2 (for high availability)
- **Resources**:
  - **Requests**: Defined to ensure guaranteed resources
  - **Limits**: Defined to cap resource usage
- **Exposed Port**: 3000

## Service Configurations

### 1. Frontend Service
- **Selector**: Targets pods with the label `app: frontend`
- **Exposed Port**: 80

### 2. Backend Service
- **Selector**: Targets pods with the label `app: backend`
- **Exposed Port**: 80 (mapped to container port 3000)

## Ingress Configuration
The Ingress resource handles domain-based routing for external traffic:
- **`app.example.com`**: Routes traffic to the frontend service
- **`api.example.com`**: Routes traffic to the backend service
- **Annotations**:
  - `nginx.ingress.kubernetes.io/rewrite-target: /` ensures proper path-based routing
- **Ingress Class**: Specifies `nginx` to use the Nginx Ingress Controller

## Pre-requisites and Deployment Steps

### Step 1: Install Nginx Ingress Controller
Run the following command to deploy the Nginx Ingress Controller in your Kubernetes cluster:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml
```

### Step 2: Apply Deployment Configurations
Deploy the application resources:
```bash
kubectl apply -f domain-routing.yaml
```

### Step 3: Verify the Deployment
Check the status of deployments, services, and ingress:
```bash
kubectl get deployments
kubectl get services
kubectl get ingress
```

## Additional Considerations
- **Container Images**: Replace `frontend-react-app:latest` and `backend-nodejs-api:latest` with actual image names.
- **SSL/TLS**: Configure SSL/TLS certificates for secure routing. Use tools like [cert-manager](https://cert-manager.io/) for automated certificate management.
- **Health Checks**: Add liveness and readiness probes in deployment configurations to ensure proper health monitoring.
- **Environment Configurations**: Use ConfigMaps and Secrets for environment-specific settings.

## Folder Structure
```
.
├── frontend-deployment.yaml
├── backend-deployment.yaml
├── services.yaml
├── ingress.yaml
└── README.md
```

## License
This repository is available under the [MIT License](LICENSE).