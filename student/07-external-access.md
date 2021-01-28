# Exposing Services Outside The Cluster

## Introduction

While you can access the voting app using port-forwarding, this isn't very efficient as almost all users won't have `kubectl` installed (and won't have access to the cluster!). So how can you expose your service to external users (meaning outside your cluster)?

You can update your front-end service to be a `LoadBalancer` type instead of the default (which is `ClusterIP`).

Once you do that, you should be able to access front-end and vote for cats and dogs!

## Success Criteria

To successfully complete this exercise, you should have:

- Update your front-end service to be of a `LoadBalancer` type
- By using the created public IP address, access your voting app
- Successfully vote for cats or dogs!

## Background

### Service Type

By default, your service specifies a [ServiceType](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types). The default value is `ClusterIP`, which exposes your service on an internal IP inside your cluster. The cluster ip is not reachable outside the cluster by default.

You can update your `ServiceType` and then apply your YAML.

Also, take a look at the generated Public IP resource that's created in your MC_xxx resource group that's tied to your AKS cluster.

Congratulations - you've exposed your service to external users via a public IP address!
