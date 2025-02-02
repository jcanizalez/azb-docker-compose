# azb-docker-compose

## Docker Compose (H2 + Azure Storage)

### Step 1 - Register Azure Application

Please follow this steps:

* Create Azure Application.
* Enable the mobile and desktop flows. (Authentication + Advance Settings)
* Create Application URI Id inside "Expose an API". (Example: api://azbuilder)
* Add API scope. (Example: api://azbuilder/Builder.Default)
* Add an application role. (Example: Builder.Application.Default)
* Add the API permission as an application permission.
* Create an application secret.

For more information visit:
* [Register an application with the Microsoft identity platform](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app)
* [Configure a client application to access a web API](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-configure-app-access-web-apis)
* [Configure an application to expose a web API](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
* [Azure OAuth 2.0 Sample for Azure AD Spring Boot Starter Resource](https://github.com/Azure/azure-sdk-for-java/tree/main/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-active-directory-resource-server#configure-web-api)

### Step 2 - Build Docker Images
In order to use this docker-compose you will need to clone and build the following repositories locally:

* [https://github.com/AzBuilder/azb-server](https://github.com/AzBuilder/azb-server)
* [https://github.com/AzBuilder/azb-pod-creator](https://github.com/AzBuilder/azb-pod-creator)

Command to build the images: 
```bash
mvn spring-boot:build-image
```

> To compile the projects you will need to setup the access to github packages for some dependencies. To setup github packages for maven visit this [page](https://docs.github.com/en/enterprise-server@3.0/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry).


### Step 3 - Create Azure Storage Account

Create an Azure Storage account and add the following containers with private access:

| Container    | Description                         |
| -------------| ------------------------------------|
| tfoutput     | Container to store terraform output |
| tfstate      | Container to store terraform state  |

## Step 4 - Docker Compose Environment Variables

Fill environment variables for api-server inside docker-compose.yaml:

| Component       | Description                         |
| ----------------| ------------------------------------|
| AzureAdAppId    | Azure Application (Client Id)       |

Fill evironment variables for api-job inside docker-compose.yaml:

| Component              | Description                   |
| -----------------------| ------------------------------|
| AzureAdAppClientId     | Azure application (Client Id) |
| AzureAdAppClientSecret | Azure application secret      |
| AzureAdAppTenantId     | Azure tenant Id               |

Fill environment variables for terraform executor inside docker-compose.yaml:

| Component                             | Description                          |
| --------------------------------------| ------------------------------------ |
| AzureTerraformStateResourceGroup      | Azure storage account resource group |
| AzureTerraformStateStorageAccountName | Azure storage account name           |
| AzureTerraformStateStorageAccessKey   | Azure storage account access key     |
| AzureTerraformOutputAccountName       | Azure storage account name           |
| AzureTerraformOutputAccountKey        | Azure storage account key            |
| AzureAdAppClientId                    | Azure application (Client Id)        |
| AzureAdAppClientSecret                | Azure application secret             |
| AzureAdAppTenantId                    | Azure tenant Id                      |

> You can use one the same storage account for the terraform state and output or you can use different storage accounts

## Step5 - Run AzBuilder Platform

```bash
docker-compose up
```