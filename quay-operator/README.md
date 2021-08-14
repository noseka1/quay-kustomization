# Quay Operator

This kustomization creates a subscription to the latest version of [quay-operator](https://github.com/redhat-cop/quay-operator) and deploys an instance of this operator on OpenShift.

To apply the kustomization run this command as a user with a *cluster-admin* role:

```
$ oc apply --kustomize base
```
