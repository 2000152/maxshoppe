# Maxshoppe: An Express Node.js App Deployment Guide on Azure with MSSQL

This guide outlines the steps for constructing and deploying an Express Node.js application, Maxshoppe, that includes an MSSQL database on Azure through Docker containers.
Deployment URL : https://maxshoppe.azurewebsites.net/
## Pre-requisites

- Docker installation on your local machine
- Azure CLI installation on your local machine
- Valid Azure account with an active subscription

## Step 1: Construction of the Docker Image

Switch to the directory where your Dockerfile resides and execute the following command to create the Docker image:

```bash
docker build -t maxshoppe:v1 .
```

## Step 2: Establishing Connection to Azure and Azure Container Registry (ACR)

Authenticate your access to Azure:

```bash
az login
```

Proceed with the Azure Container Registry (ACR) login:

```bash
az acr login --name maxshoppeacr
```

## Step 3: Assigning Tags to the Docker Image

Tag the Docker image with your ACR's login server:

```bash
docker tag maxshoppe:v1 maxshoppeacr.azurecr.io/maxshoppe:v1
```

## Step 4: Uploading the Docker Image to ACR

Push your Docker image to the ACR:

```bash
docker push maxshoppeacr.azurecr.io/maxshoppe:v1
```

## Step 5: Creating Resource Group and MSSQL Server Container on Azure

Initialize a resource group:

```bash
az group create --name MaxShoppeResourceGroup --location eastus
```

Establish an Azure Container Instances (ACI) for the MSSQL Server:

```bash
az container create --name mssqlservercontainer --resource-group MaxShoppeResourceGroup --image mcr.microsoft.com/mssql/server:2022-latest --ip-address Public --ports 1433 --cpu 2 --memory 4 --environment-variables ACCEPT_EULA=Y SA_PASSWORD=Password@123
```

## Step 6: Launching the Application Container on Azure

Set up a Web App for Containers service on Azure:

```bash
az appservice plan create --name MaxShoppeServicePlan --resource-group MaxShoppeResourceGroup --sku B1 --is-linux

az webapp create --resource-group MaxShoppeResourceGroup --plan MaxShoppeServicePlan --name maxshoppeapp --deployment-container-image-name maxshoppeacr.azurecr.io/maxshoppe:v1
```

## Step 7: Configuration of Advanced Settings 

Navigate to the properties of the web app and in the 'Advanced' tab, include the following JSON configuration:

```json
{
    "name": "DB_HOST",
    "value": "ip_address",
    "slotSetting": false
},
{
    "name": "DB_NAME",
    "value": "maxshoppe",
    "slotSetting": false
},
{
    "name": "DB_PASSWORD",
    "value": "Password@123",
    "slotSetting": false
},
{
    "name": "DB_USER",
    "value": "sa",
    "slotSetting": false
}
```

## Step 8: Configuring Environment Variables

Set the necessary environment variables for the application:

```bash
az webapp config appsettings set --resource-group MaxShoppeResourceGroup --name maxshoppeapp --settings DB_HOST=ipaddress DB_NAME=maxshoppedb DB_PASSWORD=Password@123 DB_USER=sa
```

## Step 9: Finalizing the Deployment

Finally, open the Azure portal and locate the web application at https://maxshoppe.azurewebsites.net/
