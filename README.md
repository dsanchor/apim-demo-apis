# Azure API demos for APIM

## Overview

This repository contains full APIs with policies and all the content required for an APIM demo.


## Prerequisites

This demo requires the following prerequisites:
- Have an APIM previously deployed and configured. You can follow the steps described in this repository: [apim-demo-infra](https://github.com/dsanchor/apim-demo-infra)
- Fork this repository and clone it locally. 
- An Azure subscription
- The Azure CLI
- A Service Principal with Contributor rights on the subscription
- Setup credentials in Github Secrets
- Create an storage account and a container for the temporary files used in deployment

See next sections for instructions on how to set up these prerequisites.

### Fork and clone the repository

Fork this repository first. 
Then, clone it locally by running the following command in the directory where you want to have the repository:

```bash
git clone <your_repository>.git
```

Move to the *dev* branch:

```bash
git checkout dev
```

We will use the *dev* branch to make changes to the infrastructure which will be deployed as the Develpoment environment. The *main* branch will be used to deploy the Production environment after the changes have been tested in the Development environment, create a PR from the *dev* branch to the *main* branch and merge it.


### Azure subscription

You must have an Azure subscription to deploy this demo. If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free).

### Azure CLI

We will create some prerequired resources with the [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### Service Principal

This demo needs a Service Principal to deploy the infrastructure. You can create a Service Principal with the following instructions:

- Log in to Azure:

```bash
az login
```

- List the available subscriptions:

```bash
az account list -o table
```

- Init the SUBSCRIPTION_ID variable with the subscription ID you want to use:

```bash
export SUBSCRIPTION_ID=<subscriptionID>
```

- Create the Service Principal with contributor rights on the subscription:
    
```bash
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID" --sdk-auth
```

Copy the json output of the command and keep it safe for next step (JSON_SP_FOR_GITHUB).

For details, see the instructions in the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest) to create a Service Principal.

### Github Secrets

We will create the following secrets in Github Secrets, where all values are the ones you got from the previous step:

- AZURE_CREDENTIALS = <JSON_SP_FOR_GITHUB>

### Storage account and container

We will use a storage account and a container to store temporary data. You can create a storage account and a container with the following instructions:

- Init the RESOURCE_GROUP variable with the name of the resource group you want to use:

```bash
export RESOURCE_GROUP=<resourcegroupname>
```

- Create the resource group:

```bash
az group create --name $RESOURCE_GROUP --location westeurope --subscription $SUBSCRIPTION_ID
```

- Init the STORAGE_ACCOUNT_NAME variable with the name of the storage account you want to use:

```bash
export STORAGE_ACCOUNT_NAME=<storageaccountname>
```

- Create the storage account:

```bash
az storage account create --name $STORAGE_ACCOUNT_NAME --resource-group $RESOURCE_GROUP --location westeurope --sku Standard_LRS --subscription $SUBSCRIPTION_ID
```

- Under *.github/workflows/* directory, find the *apis-deployment.yaml* file and modify the name of the variables:


RG: Name of the resource group where storage account and apim service is located.

APIM_SERVICE: Name of the APIM service instance

SA: Name of the storage account created in previous step


# Run the automation

We have included a [GitHub Action](.github/workflows/apis-deployment.yaml) to run the apis deployment.

This automation will register APIs inside APIM and its related policies.

To run the automation, push the changes to the *dev* branch. The automation will run automatically.

```bash	
git add .
git commit -m "Initial commit"
git push origin dev
```