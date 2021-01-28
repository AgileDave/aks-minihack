# Namespaces

## Overview

In this exercise, you'll take what you built in the previous exercise, but deploy it to a separate [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/). This will include your pod and PVC.

You'll keep your NGINX and PVC running from the previous exercise. We'll create new Pod and PVC resources.

## Success Criteria

To successfully complete this exercise, you should:

- Create a new namespace via a YAML file
- Deploy a new PVC to this namespace
- Deploy a new NGINX pod to this namespace, using the PVC in that namespace

## Background

### How to Create a Namespace

You can create a namespace from the command line using `kubectl`:

```
kubectl create namespace <namespace-name> --dry-run=client -o yaml > hack-ns.yaml
```

Deploy your namespace via `kubectl`.

Deploy your Pod and PVC YAML manifests to the new namespace that you just created. `kubectl` has a `-n` flag that lets you do that.

When completed, you should be able to connect to your new NGINX pod using a `port-forward` command, and you should see a new Azure Files container created that has your `index.html` file.
