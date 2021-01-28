# Provision an AKS Cluster

## Create Resource Group for AKS Cluster

In the Azure portal

- Select Create Resource
- Type and select 'resource group'
- Click the Create button for a new Resource Group
- Provide a name for your Resource Group along with selecting a Region (e.g. East US)
- Optionally, provide tags for your resource group
- On the Review and Create screen, click Create
- After 30 seconds, your new resource should be available. Navigate to it.

## Create AKS Cluster Resource

In the Azure portal while you're viewing your new resource group,

- Select the "+ Add" button
- Search for "Kubernetes Service" and select it
- On the Kubernetes Service page, select Create
- Provide the necessary information on the Basics tab, such as:
  - Resource group this cluster will be deployed to (should be filled in)
  - Cluster name
  - Region (should be resuorce group region)
  - Version (select 1.19.3)
  - Node size (we'll choose a smaller size, such as B2s)
  - Keep the node count at 3
- Select 'Next: Node Pools', and keep the defaults.
- Select 'Next: Authentication', and make this change:
  - Change Authentication Method to Service Principal
  - Allow Azure to create a new Service Principal for you
- Select 'Next: Networking', and keep all defaults
  - Set the 'DNS Name Prefix' if it is not already filled out
- Select 'Next: Integrations', and keep all defaults
- Select 'Next: Tags', and add any optional tags for this resource
- Select 'Next: Review and Create', review your values, and then click Create
- It will take about 3-5 minutes for your cluster to be created

## Enable Your AKS Cluster to Have Access to Your ACR

In order for your AKS cluster to pull images from your ACR, you'll have to grant the 'AcrPull' role to the service principal that your cluster runs under. If you chose to use managed identity, then you can attach your ACR via the portal provisioning process for AKS cluster creation.

[This link](https://docs.microsoft.com/en-us/azure/aks/cluster-container-registry-integration) provides information about the process of attaching an ACR to your AKS.

In short, the command you are issuing is the following:

```
az aks update -g <AKS-resource-group> -n <AKS-cluster-name> --attach-acr <ACR-name>
```

## (Optional) Install Kubernetes Client Tool (kubectl)

**This step is optional**
_Your Azure Cloud Shell already has `kubectl` installed, and you can run all commands from the Azure Cloud Shell_

After your cluster has been created, you can install the `kubectl` Kubernetes client tool on your local workstation by running the following command:

```
az aks install-cli
```

After this has completed, you can verify that `kubectl` has been install by typing

```
kubectl version
```

You should see output about the client version, and unknown server version information.

## Set context in kubectl for AKS Cluster

From a command line where your `kubectl` tool is installed, fetch your AKS connection information by typing:

```
az aks get-credentials -n <aks-cluster-name> -g <resource-group-name>
```

This will download your cluster connection information to a configuration file that `kubectl` can read.

This file is generally found in your $HOME directory, under either a .kube or .azure-kube directory, named `config`. Open the config file up and take a loook at its contents.

## Quick test

Let's ensure we can connect to our cluster. Using `kubectl` fetch your cluster node information by typing:

```
kubectl get nodes
```

You should see information about your 3-node cluster output.

Try to deploy a simple pod to your cluster by typing:

```
kubectl run nginx --image=nginx --restart=Never
```

After several seconds, check out the running pods in your cluster by typing:

```
kubectl get pods
```

And you should see an 'nginx' pod in a Running state.

Clean up this pod by deleting it by issuing this command:

```
kubectl delete pod nginx
```

After several seconds, the pod should no longer be listed. Verify by typing:

```
kubectl get pods
```

The nginx pod should not be listed.

## Congrats!

Congratulations! You have a functioning AKS Cluster!!
