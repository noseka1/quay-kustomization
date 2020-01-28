# Kustomization for Installing Red Hat Quay on OpenShift

This repository contains kustomizations for installing Red Hat Quay on OpenShift. There are three separate kustomizations available each implementing a part of the Quay deployment:

* [quay-operator](quay-operator) Installs an operator for deploying and managing Quay instances.
* [quay-instance](quay-instance) Uses the Quay operator installed in the previous step to deploy an instance of Red Hat Quay. Must be deployed to the same OpenShift project as the quay-operator.
* [container-security-operator](container-security-operator) Installs Container Security Operator. This operator injects information on detected vulnerabilities to Kubernetes/OpenShift. Installation of this operator is *optional*. There's one instance of this operator installed per OpenShift cluster.

# Quick Start

Note that some of the following commands require *cluster-admin* role.

Create a new project called `quay-enterprise`:

```
$ oc new-project quay-enterprise
```

Install Quay operator into the `quay-enterprise` project:

```
$ oc apply --kustomize quay-operator/base
```

Refer to [Accessing Red Hat Red Hat Quay](https://access.redhat.com/solutions/3533201) to get the credentials to pull containers from the Quay.io registry. Save the credentials as `quay-instance/base/redhat-quay-pull-secret.json`.

Deploy a Red Hat Quay instance into the `quay-enterprise` project:
 
```
$ oc apply --kustomize quay-instance/overlays/development
```

Optionally, install Container Security Operator:

```
$ oc apply --kustomize container-security-operator/base
```
