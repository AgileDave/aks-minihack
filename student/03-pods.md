# Create a Pod in AKS

## Overview

In this exercise, we'll create an NGINX pod that will use Azure Files for its underlying content storage. As part of this exercise, we'll create the following Kubernetes objects:

- Pod
- StorageClass
- Persistent Volume Claim (PVC)
- InitContainers
- Liveness Probe

We need to make sure that the NGINX image has a "Hello World" HTML file that we can browse to. And we also need to ensure that the pod is in a healthy state able to receive traffic.

## Success Criteria

To complete this exercise, we'll need to:

- Deploy the NGINX image to your AKS cluster
- Mount a volume that is hosted in Azure Files to the NGINX image
- Initialize the volume with a simple "Hello world" HTML file using an InitContainer
- Ensure that the pod is "alive" by using a Liveness Probe. The probe should check that the "Hello World" HTML file exists. If the file exists, the probe returns true; false otherwise.
- Using port-forwarding, connect to your pod to verify the HTML file can be retrieved via a web browser.

## BACKGROUND: Create a Basic Pod YAML file

Using your `kubectl` command line tool, we can create templates for pod definitions. Instead of using `kubectl` to deploy a pod to Kubernetes, we can use it to generate a YAML file, as such:

```
kubectl run nginx --image=<your-acr>.azurecr.io/nginx --restart=Never --dry-run=client -o yaml > nginx-pod.yaml
```

Notice that nothing gets created in your AKS cluster, but a file is create where you ran this command. Inspect it.

You can deploy this YAML file by issuing an `apply` command:

```
kubectl apply -f nginx-pod.yaml
```

Check the status of your pod by issuing a `get pods` command:

```
kubectl get pods
```

You can connect to this pod by issuing a `port-forward` command:

```
kubectl port-forward <pod-name> 8888:80
```

You can then browse to the pod in your browser by going to `localhost:8888`. You should see the default NGINX page. [Port-forwarding](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) creates a tunnel from your machine to a pod, deployment, or service in your cluster. This is helpful in debugging and testing what you've deployed to AKS.

You can then delete this pod by issuing a `delete` command:

```
kubectl delete -f nginx-pod.yaml
```

Check the status of your pods and notice that the nginx pod has been removed.

## Hack Steps

### 1. Create the NGINX pod

Using info from the Background section, create a YAML file for your NGINX pod. You can deploy this pod to test that it is valid and successfully deploys your pod.

### 2. Create a Persistent Volume Claim for Azure Storage

For our hacks, we'll create a dynamic Persistent Volume Claim (PVC) using Azure Files. This PVC will dynamically create the Persistent Volume (PV) and also create the Azure Storage account behind the scenes. Check out the docs on this [here](https://docs.microsoft.com/en-us/azure/aks/azure-files-dynamic-pv).

Deploy your StorageClass and PVC to your AKS cluster.

Modify your NGINX yaml file to create a volume that mounts the PVC you just created. Choose a mount path like `/usr/share/nginx/html`, which is the default content location for NGINX.

## 3. Set Up an InitContainer

Modify your NGINX yaml file to include an [InitContainer](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) that will create the "Hello World" HTML file and save it to your Azure Files account. You can do this by issuing a `command` in your InitContainer definition.

## 4. Create a Liveness Probe

Modify your NGINX yaml file so that you use a [Liveness Probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) to determine if your NGINX pod is healthy. Your liveness probe should be an HTTP request probe.

## 5. Test your app

Connect to your pod by using a `port-forward` command from `kubectl`. Choose any port that you want to use to connect to your pod (e.g. 8888); your NGINX pod should be running on port 80.

## 6. BONUS

Finished early? Try going to the Azure Files container and find the `index.html` file that was created by the busybox image. Modify that file if you want, but also try crafting a few other files and uploading them to your Azure Files folder. Can you link these pages together so that you can navigate these pages via your browser?

Lastly, if you delete the pod and then `apply` it again using `kubectl`, what will happen to your `index.html` file?
