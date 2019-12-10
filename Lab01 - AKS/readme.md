# Lab01 - AKS (Azure Kubernetes Service)
![aks-logo](./img/aks.svg)
This lab aims to provide a hands-on experience in 
pushing container images to ACR and deploying containerized workloads using AKS.

## Prequisites

> **Important**
> 
> The following prequisites are required in order to execute the lab successfully:
>
> - Azure CLI
> - Docker
> - VS Code

## About the exercise

### Signing in to Azure using the Azure CLI

In order to create an Azure CLI session, please sign in using the following Azure CLI command line, from your preferred shell.

```bash
az login
```

When prompted for credentials, please sign in using your lab credentials.

### Table of Contents

The exercise is divided into the following parts:
- [AKS Deployment]()
- [ACR Deployment]()
- [Pushing a container image to ACR]()
- [Deploying a containerized workload using AKS]()