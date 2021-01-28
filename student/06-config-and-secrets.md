# Updating Deployments with New Images, ConfigMaps, and Secrets

## Overview

Now that we've deployed our voting app, a few things have changed. We need to do the following things:

- We need to update the version of the front-end image to v2 (it's currently at v1)
- We need to extract the environment variable in the front-end deployment into a Secret and reference that Secret in the front-end deployment
- We need to extract the environment variable in the back-end deployment into a ConfigMap and reference that ConfigMap in the back-end deployment
- Once we've done all that, our management tells us that they like the v1 version of the voting app and want us to roll back to v1

## Success Criteria

To successfully complete this exercise, you should be able to:

- Update the front-end deployment version to use v2 of the azure-vote-front image. (You should have imported this into your ACR in the first exercise.)
- Create a [Kubernetes Secret](https://kubernetes.io/docs/concepts/configuration/secret/), storing the "REDIS" value in a secret named "back-end-url"
- Remove the hard-coded reference to the value of "REDIS" in the front-end deployment YAML and replace it with a secretKeyRef reference
- Create a [Kubernetes ConfigMap](https://kubernetes.io/docs/concepts/configuration/configmap/), storing the "ALLOW_EMPTY_PASSWORD" value in a configMap named "back-end-cm"
- Remove the hard-coded reference to the value of "ALLOW_EMPTY_PASSWORD" in the back-end deployment YAML and replace it with a configMapRef reference.
- Update your front-end deployment YAML to use v1 of the azure-vote-front image.
- Use the `kubectl rollout history` command to view the deployment history of the front-end

## Background

### 1. Create a Secret

You can find docs on creating a Kubernetes Secret [here](https://kubernetes.io/docs/tasks/configmap-secret/managing-secret-using-kubectl/), and referencing a secret in a deployment [here](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables).

### 2. Create and Use a ConfigMap

You can find docs on creating a Kubernetes ConfigMap object [here](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#create-configmaps-from-literal-values), and referencing a ConfigMap value in a deployment [here](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#define-container-environment-variables-using-configmap-data).
