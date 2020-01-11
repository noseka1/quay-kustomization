# Quay Instance

## Configuration

Edit [base/kustomization.yaml](base/kustomization.yaml) and set the credentails for various Quay components.

Edit [base/redhat-quay-pull-secret.json](base/redhat-quay-pull-secret.json) and insert your Red Hat Quay pull secret. This secret allows one to pull images from `quay.io/redhat`. You can obtain the Red Hat Quay secret by following the instructions at [Accessing Red Hat Quay (formerly Quay Enterprise) without a CoreOS login](https://access.redhat.com/solutions/3533201), section "JSON config.json for Red Hat Quay v3". The secret is a JSON snippet that can be directly inserted into the `.dockerconfigjson` field of a Kubernetes Secret object.

Note that the `production` overlay places Quay pods on nodes labeled with `node-role.kubernetes.io/infra: ''`. Make sure that in your OpenShift cluster you have nodes having this label attached. Alternatively, you can change or remove this label from the [overlays/production/quay-quayecosystem.yaml](overlays/production/quay-quayecosystem.yaml) descriptor.

### Registry Storage

#### Local Storage (Non-Prod Use Only)

By default Quay uses a Local storage which means that it stores container images into a directory `/datastorage/registry` on the local file system. If you mount a PVC into this directory, you will obtain a persistent local storage and images uploaded to the registry will survive a restart of the Quay pod. Note that *Local storage is not meant to be used for production deployments*.

#### Production Storage Backends

For production deployments, you should configure Quay to use one of the production-grade storage backends like S3, Blob storage (Azure, GCP), RADOS, Swift, ... See [Registry Storage](https://github.com/redhat-cop/quay-operator/blob/master/docs/storage.md) for details on the available storage backend types.

## Deployment

```
$ oc new-project quay-enterprise
```

```
$ oc apply --kustomize overlays/development
```
