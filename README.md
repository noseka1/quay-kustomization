# Kustomization for Installing Red Hat Quay on OpenShift

This repository contains kustomizations for installing Red Hat Quay on OpenShift. There are three separate kustomizations available each implementing a part of the Quay deployment:

* [container-security-operator](container-security-operator) Installs Container Security Operator. This operator injects information on detected vulnerabilities to Kubernetes/OpenShift. Installation of this operator is *optional*.
* [quay-operator](quay-operator) Installs an operator for deploying and managing Quay instances.
* [quay-instance](quay-instance) Uses the Quay operator to deploy an instance of Red Hat Quay.

# Quick Start

The following commands require *cluster-admin* role.

Install Container Security Operator:

```
$ oc apply --kustomize container-security-operator/base
```

Create a new project called *quay-enterprise*:

```
$ oc new-project quay-enterprise
```

Install Quay operator and deploy a Quay instance in the *quay-enterprise* project:

```
$ oc apply --kustomize quay-operator/base
```

```
$ oc apply --kustomize quay-instance/overlays/development
```
