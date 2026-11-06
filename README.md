# Cars Application - Argo CD Deployment

## Technologies
- AWS
- Kubernetes
- Docker
- Docker Hub
- Argo CD
- GitHub

## Application Details
- Application: Cars
- Image: satishpolaka/cars:v1
- Initial Replicas: 3
- Service: LoadBalancer
- Deployment Strategy: RollingUpdate

## Kubernetes Resources
1. Namespace
2. Deployment
3. Service
4. ConfigMap
5. ResourceQuota
6. Horizontal Pod Autoscaler

## Deployment Approach
GitOps using Argo CD.

Argo CD monitors the GitHub repository and
synchronizes Kubernetes resources automatically.