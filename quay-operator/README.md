# Quay Operator

This kustomization creates a subscription to the latest version of [Quay operator](https://github.com/redhat-cop/quay-operator) and it deploys an instance of this operator on OpenShift.

Kustomization deploys to the target namespace called `quay-enterprise`. It assumes that this namespace already exists. You can change the target namespace with:

```
$ cd base/
$ kustomize edit set namespace <namespace>
```
