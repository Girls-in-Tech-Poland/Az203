# Lab02 - Azure App Services

| App Service Plan                                | App Service                           |
| ----------------------------------------------- | ------------------------------------- |
| ![app-service-plan](./img/app-service-plan.svg) | ![app-service](./img/app-service.svg) |

This lab aims to provide a hands-on experience in
deploying applications to Azure App Services running on both Linux and Windows operating systems.

## Prequisites

### Required Resource Providers

```bash
az provider register --namespace 'Microsoft.Storage'
az provider register --namespace 'Microsoft.Web'
```

## Table of Contents

The exercise is divided into the following parts:

| Part no. | Title                                              | Expected Duration |
| -------- | -------------------------------------------------- | ----------------- |
| 1        | [Windows App Service](./01.windows-app-service.md) | 20m               |
| 2        | [Linux App Service](./02.linux-app-service.md)     | 20m               |
