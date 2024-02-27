# React Web App with Node.js API and MongoDB on Azure

## Description: 
A complete ToDo app on Azure App Service with Node.js API and Azure Cosmos API for MongoDB for storage. 

Content:
You can open the table of content by moving the cursor over the three lines in the top left corner of this Readme file window. 

## 0. Environment setup
### 0.0. Prerequisites

- [Azure Developer CLI](https://aka.ms/azd-install)
- [Node.js with npm (18.17.1+)](https://nodejs.org/) - for API backend and Web frontend

I use Azure Developer CLI (azd) for streamlined provisioning and deployment to Azure. It has the following features: 

- simple commands for workflow steps
- a library of reusable, extensible templates that are pre-configured for common scenarios
- the ability to convert existing projects into new azd templates
- portable, reusable infrastructure as code scaffolding for the project
- compatibility and support for CI/CD pipelines and monitoring


---
<!-- YAML front-matter schema: https://review.learn.microsoft.com/en-us/help/contribute/samples/process/onboarding?branch=main#supported-metadata-fields-for-readmemd -->

### 0.1. Application Code

I use template for an interctive Web App with Node.js API and MongoDB on Azure. It is structured to follow the [Azure Developer CLI](https://aka.ms/azure-dev/overview).

The blueprint includes sample application code (a ToDo web app). I modify it slightly and then leverage the Infrastructure as Code assets (written in Bicep) to create a fully functional web app.

For specifics please see file: [setup](0_env_setup.ipynb)

## 1. Provision and Deploy the Application Architecture

### 1.1. Breakdown of the process in three steps
We need to package the app, provision the necessary infrastructure and deploy the web app. This can be initiated with one command `azd up` 

The `azd up` command runs the three separate commands in sequence:
1. `azd package` for packing web app 
2. `azd provision` for provisioning infrastructure
3. `azd deploy` for deploying web app hosted on provisioned infrastructure

For specifics and images check this file: [provision](1_node_app_deploy.ipynb)

### 1.2. Architecture Components

This application utilizes the following Azure resources:

- [**Azure App Services**](https://docs.microsoft.com/azure/app-service/) to host the Web frontend and API backend
- [**Azure Cosmos DB API for MongoDB**](https://docs.microsoft.com/azure/cosmos-db/mongodb/mongodb-introduction) for storage
- [**Azure Monitor**](https://docs.microsoft.com/azure/azure-monitor/) for monitoring and logging
- [**Azure Key Vault**](https://docs.microsoft.com/azure/key-vault/) for securing secrets

Here's a high level architecture diagram that illustrates these components. Notice that these are all contained within a single [resource group](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal).

!["Application architecture diagram"](assets/resources.png)

## 2. Monitoring options

There are three monitoring dashboards: overview, live metrics and logs. See the [Monitoring Option](2_monitoring_options.ipynb) for specifics and demo images.

## 3. Customise web application code 
In this project, website owner wants to use the interactive template for tracking customer tickets.

I use text editor, Vim, to change the source code and then a granular `azd deploy` command to edit text in the web app without affecting the underlying infrastructure. This saves time. For specifics, see [Edit source code](3_customise_source_code.ipynb).

It is also possible to change the code by updating the whole environment with the`azd up` command.

!["Screenshot of deployed ToDo app"](assets/web.png)

<sup>Screenshot of the deployed ToDo app</sup>

## 4. Customise infrastructure code 

The company also needs to provide functionality for customers to upload their documents. So current infrastructure does not have any storage allocated. 

I edit infrastructure code files with Vim editor and then use the granular `azd provision` to change infrastructure without executing application code. For specifics, see [Edit IaaC](4_customise_infrastructure.ipynb)  

I can also update the whole environment by running the`azd up` command again.

## 5. Configure a CI/CD Pipeline

In the previous steps, the workflows were managed by individual commands. I now automate these workflows with a CI/CD pipeline.

### 5.1. Setup pipeline in Github

For specifics, see [CI/CD](5_cicd.ipynb).

 #######################################################
monitor your application, test and debug locally

> Note: Needs to manually install [setup-azd extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.azd) for Azure DevOps (azdo).

- [`azd pipeline config`](https://learn.microsoft.com/azure/developer/azure-developer-cli/configure-devops-pipeline?tabs=GitHub) - to configure a CI/CD pipeline (using GitHub Actions or Azure DevOps) to deploy the application whenever code is pushed to the main branch. 

- [`azd monitor`](https://learn.microsoft.com/azure/developer/azure-developer-cli/monitor-your-app) - to monitor the application and quickly navigate to the various Application Insights dashboards (e.g. overview, live metrics, logs)

- [Run and Debug Locally](https://learn.microsoft.com/azure/developer/azure-developer-cli/debug?pivots=ide-vs-code) - using Visual Studio Code and the Azure Developer CLI extension

- [`azd down`](https://learn.microsoft.com/azure/developer/azure-developer-cli/reference#azd-down) - to delete all the Azure resources created with this template 

- [Enable optional features, like APIM](./OPTIONAL_FEATURES.md) - for enhanced backend API protection and observability

## Security

### Roles

This template creates a [managed identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) for the app inside the Azure Active Directory tenant. It is used to authenticate the  app with Azure and other services that support Azure AD authentication like Key Vault via access policies. 

### Key Vault

This template uses [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/overview) to securely store the Cosmos DB connection string for the provisioned Cosmos DB account. Key Vault is a cloud service for securely storing and accessing secrets (API keys, passwords, certificates, cryptographic keys) and makes it simple to give other Azure services access to them. 

## 6. Tools
Vim text editor, Azure Developer CLI, GitHub. 