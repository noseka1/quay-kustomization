# Quay Operator

This kustomization creates a subscription to the latest version of [Quay operator](https://github.com/redhat-cop/quay-operator) and deploys an instance of this operator on OpenShift.

Kustomization deploys to the target namespace called `quay-enterprise`. It assumes that this namespace already exists. You can change the target namespace with:

```
$ pushd base
$ kustomize edit set namespace <namespace>
$ popd
```

To apply the kustomization run:

```
$ oc apply --kustomize base
```
