# Quay Instance

This kustomization uses [quay-operator](https://github.com/redhat-cop/quay-operator) to install an instance of Red Hat Quay on OpenShift. Before deploying Quay, checkout out the configuration steps in the next section.

## Configuration

Common configuration:

* This kustomization deploys Quay to the target namespace called `quay-enterprise`.

### Development Variant

Development variant uses a local storage. This means that Quay will store container images into a directory `/datastorage/registry` on the local file system. The kustomization mounts a PVC into this directory to achieve a persistent local storage. Images uploaded to the registry will survive a restart of the Quay pod. Note that *local storage is not meant to be used for production deployments*.

Development variant deploys a single Quay replica, as the local storage cannot be shared between multiple replicas.

## Deployment

To deploy Red Hat Quay for development purposes, run this command:

```
$ oc apply --kustomize overlays/development
```
