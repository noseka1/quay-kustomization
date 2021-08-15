# OBSOLETE: Quay Instance

This kustomization uses [quay-operator](https://github.com/redhat-cop/quay-operator) to install an instance of Red Hat Quay on OpenShift. Before deploying Quay, checkout out the configuration steps in the next section.

## Configuration

Common configuration:

* This kustomization deploys Quay to the target namespace called `quay-enterprise`.

### Development Variant

Development variant uses a local storage. This means that Quay will store container images into a directory `/datastorage/registry` on the local file system. The kustomization mounts a PVC into this directory to achieve a persistent local storage. Images uploaded to the registry will survive a restart of the Quay pod. Note that *local storage is not meant to be used for production deployments*.

Development variant deploys a single Quay replica, as the local storage cannot be shared between multiple replicas.

### Production Variant

For production deployments, Quay must be configured to use one of the production-grade storage backends like S3, Blob storage (Azure, GCP), RADOS, Swift, ... See [Registry Storage](https://github.com/redhat-cop/quay-operator/blob/master/docs/storage.md) for details on the available storage backend types and their configuration. You must edit [overlays/production/quay-quayecosystem.yaml](overlays/production/quay-quayecosystem.yaml) to add your storage backend configuration there.

## Deployment

To deploy Red Hat Quay for development purposes, run this command:

```
$ oc apply --kustomize overlays/development
```

To deploy Red Hat Quay to production, run this command:

```
$ oc apply --kustomize overlays/production
```
