# Quay Instance

This kustomization uses [Quay operator](https://github.com/redhat-cop/quay-operator) to install an instance of Red Hat Quay on OpenShift. Before deploying Quay, checkout out the configuration steps in the next section.

## Configuration

* This kustomization deploys Quay to the target namespace called `quay-enterprise`. It assumes that this namespace already exists. You can change the target namespace in [base/kustomization.yaml](base/kustomization.yaml).

* Edit [base/kustomization.yaml](base/kustomization.yaml) to set the credentails (username and password) for various Quay components. Note that for simplicity credentials are stored in the configuration file in plain text. Instead of storing unprotected credentials in git I strongly recommend that you consider using a [secretGeneratorPlugin](https://github.com/kubernetes-sigs/kustomize/blob/master/examples/secretGeneratorPlugin.md) to store your credentials externally.

* Open [base/redhat-quay-pull-secret.json](base/redhat-quay-pull-secret.json) and replace the content of this file with your Red Hat Quay pull secret. This secret allows one to pull images from `quay.io/redhat`. You can obtain the Red Hat Quay secret by following the instructions at [Accessing Red Hat Quay (formerly Quay Enterprise) without a CoreOS login](https://access.redhat.com/solutions/3533201), section *JSON config.json for Red Hat Quay v3*.

There are two kustomization variants available to you: [development](overlays/development) and [production](overlays/production). These variants differ mostly in the size of the deployed resources and the backend storage used.

### Development Variant (Non-Prod Use Only)

Development variant uses a Local storage. This means that Quay will store container images into a directory `/datastorage/registry` on the local file system. Kustomization mounts a PVC into this directory to obtain a persistent local storage. Images uploaded to the registry will survive a restart of the Quay pod. Note that *Local storage is not meant to be used for production deployments*.

Development variant deploys a single Quay pod to preserve resources.

### Production Variant

For production deployments, Quay must be configured to use one of the production-grade storage backends like S3, Blob storage (Azure, GCP), RADOS, Swift, ... See [Registry Storage](https://github.com/redhat-cop/quay-operator/blob/master/docs/storage.md) for details on the available storage backend types and their configuration. You must edit [overlays/production/quay-quayecosystem.yaml](overlays/production/quay-quayecosystem.yaml) to add your storage backend configuration there.

Note that the production variant places Quay pods on nodes labeled with `node-role.kubernetes.io/infra: ''`. Make sure that in your OpenShift cluster you have nodes having this label attached. Alternatively, you can change or remove this label from the [overlays/production/quay-quayecosystem.yaml](overlays/production/quay-quayecosystem.yaml) descriptor.

## Deployment

To deploy Red Hat Quay for development purposes, run this command:

```
$ oc apply --kustomize overlays/development
```

To deploy Red Hat Quay to production, run this command:

```
$ oc apply --kustomize overlays/production
```
