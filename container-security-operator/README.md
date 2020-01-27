# Container Security Operator

This kustomization creates a subscription to the latest version of [Container Security Operator](https://github.com/quay/container-security-operator) and deploys an instance of this operator on OpenShift.

Kustomization deploys to the target namespace called `openshift-operators`. Pods in all namespaces will be monitored for vulnerabilities.

To apply the kustomization run:

```
$ oc apply --kustomize base
```
