# Azure API demos for APIM

## Overview

This repository contains full APIs with policies and all the content required for an Azure APIM and APIOps demo.

## Prerequisites

The following prerequisites are required for this demo:
- An APIM previously deployed and configured. The steps described in this repository can be followed: [apim-demo-infra](https://github.com/dsanchor/apim-demo-infra)
- This repository must be forked and cloned locally.
- An Azure subscription
- The Azure CLI
- A Service Principal with Contributor rights on the subscription
- Credentials and Variables must be set up as GitHub Secrets

The next sections provide instructions on how to set up these prerequisites.

### Fork and clone the repository

This repository must be forked first. Then, it can be cloned locally by running the following command in the directory where the repository is desired:

```bash
git clone <your_repository>.git
```

The *main* branch will be used to make changes to the infrastructure. The changes will be deployed to the *dev* (Development) environment first. After the changes have been tested in the Development environment, they will be promoted to the prod (Production) environment via a workflow.

### Azure subscription

An Azure subscription is required to deploy this demo. If an Azure subscription is not available, a [free account](https://azure.microsoft.com/free) can be created.

### Azure CLI

Some resources will be created with the Azure CLI. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### Service Principal

A Service Principal is needed to deploy the infrastructure for this demo. A Service Principal can be created with the following instructions:

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

- Create a Service Principal with contributor rights on the subscription:
    
```bash
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID" --sdk-auth
```

The json output of the command must be copied and kept safe for next step.

For details, see the instructions in the  [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest) to create a Service Principal.

### GitHub Environments

The following environments must be created in the GitHub repository:

- dev
- prod

Follow the instructions in the [GitHub documentation](https://docs.github.com/en/actions/reference/environments) to create the environments.

### GitHub Secrets

The following secrets must be created as GitHub Secrets for each environment, values are obtained from the previous steps including the Azure APIM deployment:

- AZURE_CLIENT_ID
- AZURE_CLIENT_SECRET
- AZURE_TENANT_ID
- AZURE_SUBSCRIPTION_ID
- AZURE_RESOURCE_GROUP_NAME
- API_MANAGEMENT_SERVICE_NAME

# Run the automation

A [GitHub Action](.github/workflows/deploy-apis-with-apiops.yaml) has been included to run the apis deployment.

This automation will register APIs inside APIM and its related policies, tags and products.

To run the automation, push the changes to the *dev* or *main* branch. The automation will run automatically.

```bash	
git add .
git commit -m "Initial commit"
git push origin dev
```

## References:

- [APIOps Documentation](https://azure.github.io/apiops/apiops/0-labPrerequisites/)
- [APIOps GitHub Repository](https://github.com/Azure/apiops)