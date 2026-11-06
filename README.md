# Kubernetes Deployment Strategies Using Argo CD

## Project Overview

This project demonstrates different Kubernetes deployment strategies using AWS, Kubernetes, GitHub and Argo CD.

The main objective is to understand how application deployments, updates, traffic switching and rollbacks are handled in Kubernetes environments.

I have implemented the deployment strategies using Kubernetes YAML manifests and managed the deployment process through Argo CD using the GitOps approach.

## Technologies Used

| Technology | Purpose |
|---|---|
| AWS EC2 | Kubernetes infrastructure |
| kOps | Kubernetes cluster management |
| Kubernetes | Container orchestration |
| Docker | Container images |
| Docker Hub | Image repository |
| GitHub | Source code and YAML manifests |
| Argo CD | GitOps continuous delivery |

## Deployment Strategies Implemented

### 1. Rolling Update Deployment

Rolling Update is used to update an existing application without stopping all running pods at once.

**How I implemented it:**

- Created a Kubernetes Deployment with 3 replicas.
- Initially deployed the Cars application.
- Updated the container image in the Deployment manifest.
- Pushed the changes to GitHub.
- Argo CD detected the changes and synchronised the application.
- Kubernetes gradually replaced the old pods with new pods.

**Deployment configuration:**

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

### 2. Blue-Green Deployment

Blue-Green deployment maintains two separate application environments.

For this practical implementation:

- Blue environment: Cars application.
- Green environment: Shopping application.

**How I implemented it:**

- Created two separate Kubernetes Deployments.
- Deployed Cars in the Blue environment.
- Deployed Shopping in the Green environment.
- Created both environments with 3 replicas.
- Configured a Kubernetes Service to send traffic to Blue initially.
- Updated the Service selector to switch traffic to Green.
- Verified the traffic switching through the existing LoadBalancer.

### 3. Rollback

Rollback is used to move production traffic back to the previous application environment when the new deployment has an issue.

**How I implemented it:**

- Initially configured the Service to point to Blue.
- Switched traffic to Green.
- Changed the Service selector back to Blue.
- Argo CD synchronised the changes.
- Traffic was directed back to the previous environment.

This helped me understand how we can switch traffic back without rebuilding the previous application environment.

### 4. Recreate Deployment

Recreate deployment terminates all existing pods before creating new pods.

**How I implemented it:**

- Configured the Deployment strategy as Recreate.
- Initially used the Cars application.
- Changed the container image to the Shopping application.
- Kept the same Kubernetes Deployment name.
- Updated the GitHub manifest.
- Argo CD detected and synchronised the changes.
- Kubernetes terminated the existing pods before creating the replacement pods.

**Deployment configuration:**

```yaml
strategy:
  type: Recreate
```

During this process, application downtime is expected because the old pods are terminated before the new pods are created.

## Project Architecture

```text
                 GitHub Repository
                        |
                        |
                        v
                     Argo CD
                        |
                        |
                 Kubernetes Cluster
                        |
             -----------------------
             |          |          |
             v          v          v
          Namespace  Deployment  Service
                        |
                ----------------
                |              |
                v              v
           Application     Application
              Pods            Pods
```

## GitHub Repository Structure

```text
argocd-deployment-strategies/
│
├── README.md
├── namespace.yml
├── configmap.yml
├── deployment-recreate.yml
├── service.yml
├── resourcequota.yml
│
└── Backup Files
    ├── deployment.yml.bak
    ├── deployment-blue.yml.bak
    ├── deployment-green.yml.bak
    └── hpa.yml.bak
```

## GitOps Workflow

The deployment workflow followed in this project:

1. Created Kubernetes YAML manifests.
2. Pushed the manifests to GitHub.
3. Configured an Argo CD Application using the GitHub repository.
4. Argo CD monitors the configured Git repository.
5. When changes are committed, Argo CD detects the changes.
6. Argo CD synchronises the manifests with the Kubernetes cluster.
7. Verified application resources and deployment status through the Argo CD dashboard.

## What I Practised

- Kubernetes Deployments and ReplicaSets.
- Rolling Update and Recreate strategies.
- Blue-Green deployment.
- Application traffic switching using Kubernetes Services.
- Rollback using Service selectors.
- Kubernetes ConfigMaps and ResourceQuotas.
- GitOps workflow using Argo CD.
- Managing Kubernetes resources through GitHub.
- Monitoring deployment status through the Argo CD UI.

## Conclusion

Through this project, I got practical experience with different Kubernetes deployment strategies and understood how application updates, traffic switching and rollback work.

I also understood how Argo CD helps manage Kubernetes deployments through GitOps by keeping GitHub manifests and cluster resources synchronised.
