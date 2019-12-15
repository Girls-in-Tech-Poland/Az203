# Lab00 - Setup

## Prequisites

> **Important**
>
> The following prequisites are required in order to execute the lab successfully:
>
> - Git
> - Azure CLI

## Initializing the azure subscription

### Lab Credentials

Each lab participant will receive credentials which will grant access to the workshop environment.

> **Note**
>
> In order to run this workshop effectively, we have generated user credentials for you.

| Username                              | (i)ndex             | Password            |
| ------------------------------------- | ------------------- | ------------------- |
| `user[i]@gicworkshop.onmicrosoft.com` | Ask your instructor | Ask your instructor |

You can access the Azure Portal by navigating **[https://portal.azure.com](https://portal.azure.com)** and using your provided credentials

### Service Principal Credentials

Each lab participant will additionally receive a service principal account to aid with provisioning of services, such as AKS

| Service Principal Name | (i)ndex             | Password            |
| ---------------------- | ------------------- | ------------------- |
| `sp-user[i]`           | Ask your instructor | `TrudneHaslo1` |

### Azure Pass Subscriptions

![incognito](./img/incognito.png)

> **Important**
>
> Incognito mode is required (`Ctrl+Shift+N`)

- Open your preferred browser in incognito mode
- Navigate to [https://www.microsoftazurepass.com/](https://www.microsoftazurepass.com/)
- Sign in using your **lab credentials**
- Complete the process to activate your Azure Pass subscription

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
```

### Clone the repository into a desired folder
```
git clone https://github.com/Girls-in-Tech-Poland/Az203.git
```

## Conclusion

At this point, we assume that you have successfully used the Azure CLI to provision resource groups for the duration of the workshop and you are ready to begin the remaining labs.

## Running the workshop contents at home

The entire contents of this workshop can be done at home, at your own pace using an [Azure free account](https://azure.microsoft.com/en-us/free/).

When provisioning the free account, please don't use your main Live ID
 **For the duration of the workshop, please use the provided Azure credentials & subscription(s)**

