# Quay Bridge Operator

This kustomization creates a subscription to the latest version of [Quay Bridge Operator](https://github.com/quay/quay-bridge-operator) and deploys an instance of this operator on OpenShift.

Kustomization deploys to the target namespace called `openshift-operators`. Operator responsible for facilitating the utilization of Red Hat Quay as the default image registry for an OpenShift Container Platform environment.

To apply the kustomization run this command as a user with a *cluster-admin* role:

```
$ oc apply --kustomize base
```
