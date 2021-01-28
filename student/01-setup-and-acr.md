# Setup for AKS Mini-Hack

## 0. Log in to Azure Portal

Using the credentials provided, log in to the [Azure portal](https://portal.azure.com).

## 1. Start Azure Cloud Shell

In the upper right corner of the Azure Portal, click the icon for the Azure Cloud Shell. A panel will open in the bottom of the Azure Portal. You will be prompted to create an Azure Storage Account for your Cloud Shell. Accept the defaults.

Ensure that the Azure CLI is installed in your Cloud Shell by typing:

```
az --version
```

You should see a response stating the version (e.g. 2.18) along with additional information about extensions and dependencies.

You should also be able connected to your Azure subscription in the Cloud Shell. Verify this by typing:

```
az account show
```

If you are not connected to a subscription, login to Azure from the cloud shell:

```
az login
```

After logging in, you should be able to run `az account show` to view your subscription information.

You are now set up to use Azure Cloud Shell!

## 2. Set up an Azure Container Registry

Follow the steps in [this document](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal) to set up a **Basic** Azure Container Registry. You should also be able to log in to the ACR from the Azure Cloud Shell that we set up in the previous exercise.

## 3. Import Sample Images Into Your Azure Container Registry

From your Azure Cloud Shell prompt, we'll import a few images into our ACR. Follow the steps [here](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-import-images#import-from-docker-hub) to import the following images into your ACR. Keep the repository names the same - if you change them, you'll need to change them in later exercises to match.

- docker.io/library/nginx:latest
- docker.io/library/busybox:latest
- mcr.microsoft.com/oss/bitnami/redis:6.0.8
- mcr.microsoft.com/azuredocs/azure-vote-front:v1
- mcr.microsoft.com/azuredocs/azure-vote-front:v2

For example, to import the nginx image, you could run:

```
az acr import --name <your-acr-name> --source docker.io/library/nginx:latest --image demo/nginx:latest
```

Keep the image names and tags the same.

You should be able to go to your ACR and see that those images are now contained in your ACR.

Congratulations! You've set up an ACR and imported a few basic images!!
