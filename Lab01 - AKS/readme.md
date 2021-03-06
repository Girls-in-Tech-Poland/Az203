# Lab01 - AKS (Azure Kubernetes Service)

| AKS                        | ACR                        |
| -------------------------- | -------------------------- |
| ![aks-logo](./img/aks.svg) | ![acr-logo](./img/acr.svg) |

This lab aims to provide a hands-on experience in
pushing container images to ACR and deploying containerized workloads using AKS.

## Prequisites

> **Important**
>
> The following prequisites are required in order to execute the lab successfully:
>
> - Git
> - Azure CLI
> - NodeJS
> - Docker
> - VS Code

### Required Resource Providers

```bash
az provider register --namespace 'Microsoft.Compute'
az provider register --namespace 'Microsoft.ContainerService'
az provider register --namespace 'Microsoft.ContainerRegistry'
```

## Terms used

- `Kubernetes`: The container orchestrator used in this exercise.
- `k8s`: "k" + 8 characters + "s", a common abbreviation for `Kubernetes`.
- `AKS`: Azure Kubernetes Service, an Azure-hosted & Microsoft managed Kubernetes as a service.
- `ACR`: Azure Container Registry, an Azure-hosted private container registry.

## About the exercise

### Signing in to Azure using the Azure CLI

In order to create an Azure CLI session, please sign in using the following Azure CLI command line, from your preferred shell.

```bash
az login
```

When prompted for credentials, please sign in using your lab credentials.

### Table of Contents

The exercise is divided into the following parts:

| Part no. | Title                                                                 | Expected Duration |
| -------- | --------------------------------------------------------------------- | ----------------- |
| 1        | [AKS Deployment](./01.aks-deployment.md)                              | 15m               |
| 2        | [ACR Deployment](./02.acr-deployment.md)                              | 15m               |
| 3        | [Pushing a container image to ACR](./03.push-to-acr.md)               | 10m               |
| 4        | [Deploying a containerized workload using AKS](./04.deploy-to-aks.md) | 10m               |
