# GitOps Platform – Deployment Repository

This repository contains the Kubernetes deployment configuration for a GitOps-based deployment workflow using Helm and ArgoCD on Amazon EKS.
ArgoCD continuously monitors this repository and automatically synchronizes changes to the Kubernetes cluster.


## Tech Stack

- Kubernetes
- Helm
- ArgoCD
- Amazon EKS
- GitOps


## Architecture Flow
![Architecture](Project-9_Screenshots/1_Architecture.png)


## Repository Structure

```text
 argocd
|   |-- application.yaml
|---environments
|   |-- dev
|   |-- staging
|   |-- prod
|
|---helm
    |-- p9-app
```

### Structure Explanation

- `argocd/` → ArgoCD application manifest
- `environments/` → environment-specific Helm values
- `helm/` → Kubernetes Helm chart templates


## Final Architecture Flow
![Full Architecture of App-repo & Deploy-repo](Project-9_Screenshots/6_Full-Architecture_OF_APPrepo-and-Deployrepo.png)


## Deployment Workflow

1. Developer pushes application changes to the Application Repository
2. GitHub Actions builds a new Docker image
3. The image is pushed to Docker Hub using a SHA-based tag
4. The deployment repository is updated with the new image tag
5. ArgoCD detects changes automatically
6. ArgoCD synchronizes the Kubernetes cluster
7. Amazon EKS deploys the updated application pods


## Features

- GitOps-based continuous deployment
- Automated Kubernetes synchronization with ArgoCD
- Helm-based Kubernetes deployment management
- SHA-based Docker image versioning
- Environment-specific configuration structure
- Deployment on Amazon EKS


## Screenshots

### ArgoCD Application Status
This screenshot shows the GitOps application managed by ArgoCD after successful synchronization with the Kubernetes cluster.
![ArgoCD Application](Project-9_Screenshots/2_ArgoCD-Application-Deployment.png)


### ArgoCD Deployment Tree
This screenshot shows the Kubernetes resources managed by ArgoCD through the GitOps workflow.
Resources deployed automatically from the Git repository:
- Kubernetes Deployment (p9-app)
- Service (p9-app)
- ReplicaSet
- Running Pods
![ArgoCD Tree](Project-9_Screenshots/3_ArgoCD-Resource-Tree-View.png)


### Replica Drift Detection & Self-Healing
This screenshot demonstrates ArgoCD detecting configuration drift after manually scaling the Kubernetes Deployment from the cluster terminal.
Manual command executed:
```bash
kubectl scale deployment p9-app --replicas=5
```
What happened:
- Kubernetes temporarily created 5 running pods
- The cluster state no longer matched the desired state stored in Git
- ArgoCD detected the drift automatically
- ArgoCD reconciled the deployment back to the Git-defined replica count
- Extra pods entered the Terminating state and Kubernetes removed them automatically
#### This demonstrates GitOps self-healing behavior, where ArgoCD continuously enforces the desired configuration defined in the Git repository.
![Replica Drift Detection & Self Healing](Project-9_Screenshots/4_Replica-Drift-Detection-and-Self-Healing.png)


### Git-Driven Replica Scaling
This screenshot shows ArgoCD successfully applying a scaling change defined inside the Git repository.
The replica count was updated in the Helm values file and committed to Git:
``` replicaCount: 3 ```
![Git Driven Replica Scaling](Project-9_Screenshots/5_Git-Driven-Replica-Scaling.png)


## Related Repository
Application Repository:
https://github.com/AliYasser2003/P9_Gitops-Platform-App



## Key GitOps Concepts Demonstrated

- Continuous Deployment with ArgoCD
- Git as the single source of truth
- Kubernetes rolling updates
- Drift detection and self-healing
- Helm-based application deployment
- SHA-based Docker image versioning
- Automated synchronization between Git and Kubernetes
