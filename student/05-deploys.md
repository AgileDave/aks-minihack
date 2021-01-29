# Deployments and Services

## Overview

In this exercise, we'll create a simple voting app (the Cats vs. Dogs app) from images that you placed in your ACR in exercise 1. We'll deploy these images as a Kubernetes deployment into a separate `voting` namespace (which you'll have to create).

You'll create 2 deployment YAML files: one for the voting front end and one for the redis back end. The voting front end deployment will have 3 replicas (copies) of your voting front end image while the redis back end deployment will just have a single instance of that image.

After creating the deployments, we will create services that we can use to connect our application together along with being able to access it from outside the cluster.

## Success Criteria

In order to successfully complete this exercise, you should be able to deploy the voting front end and back end images to your cluster in the `voting` namespace. You will also have created two services: one for the front-end and one for the back-end, in the `voting` namespace. Lastly, you should be able to test this by issuing a `port-forward` command to connect to the front-end service in your cluster.

An inventory of items to complete in this exercise:

- Create `voting` namespace
- Deploy the back-end image (1 replica)
- Create a back-end service that fronts traffic to the back-end deployment
- Deploy the front-end image (3 replicas)
- Create a front-end service that fronts traffic to the front-end deployment

## Background

### Environment Variables

The pods in this exercise use a few environment variables. Check out [this doc](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) on how to set environment variables in Kubernetes manifests.

### The Front-End Deployment

There is an environment variable that needs to be set in the azure-front-end image:

```
REDIS: <service-for-your-back-end-service>
```

Keep in mind that the service should also be qualified for your `voting` namespace.

### The Back-End Deployment

There is an environment variable that needs to be set in the azure-back-end image (the redis image):

```
ALLOW_EMPTY_PASSWORD: "yes"
```

### Creating a Service

A service can be created via the command prompt using `kubectl`, as such:

```
kubectl expose deploy azure-vote-front -n voting --ports 80
```
