# Lab00 - Setup

## Prequisites

> **Important**
> 
> The following prequisites are required in order to execute the lab successfully:
>
> - Azure CLI
>

## Initializing the azure subscription

> **Note**
>
> The entire contents of this workshop can be done at home, at your own pace using an [Azure free account](https://azure.microsoft.com/en-us/free/).
>
> When provisioning the free account, please don't use your main Live ID 
>
> For the duration of the workshop, please use the provided Azure credentials & subscription(s)


## Initializing the lab resource groups

### Signing in to Azure using Azure CLI
In order to establish a secure session using Azure CLI, please use the following command.

```bash
az login
```

When prompted, please use your provided lab credentials in order to sign in.

### Provisioning lab resource groups using Azure CLI 
```bash
az group create --name lab01 --location westeurope
az group create --name lab02 --location westeurope
az group create --name lab03 --location westeurope
az group create --name lab04 --location westeurope
az group create --name lab05 --location westeurope
az group create --name lab06 --location westeurope
```

## Conclusion

At this point, we assume that you have successfully used the Azure CLI to provision resource groups for the duration of the workshop and you are ready to begin the remaining labs.
