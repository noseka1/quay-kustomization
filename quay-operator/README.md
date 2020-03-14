# Quay Operator

This kustomization creates a subscription to the latest version of [quay-operator](https://github.com/redhat-cop/quay-operator) and deploys an instance of this operator on OpenShift.

Kustomization deploys to the target namespace called `quay-enterprise`. You can change the target namespace with:

```
$ cd base/
$ kustomize edit set namespace <namespace>
$ cd ..
```

To apply the kustomization run this command as a user with a *cluster-admin* role:

```
$ oc apply --kustomize base
```
