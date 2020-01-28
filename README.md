# Kustomization for Installing Red Hat Quay on OpenShift

This repository contains kustomizations for installing Red Hat Quay on OpenShift. There are three separate kustomizations available each implementing a part of the Quay deployment:

* [quay-operator](quay-operator) Installs an operator for deploying and managing Quay instances.
* [quay-instance](quay-instance) Uses the Quay operator installed in the previous step to deploy an instance of Red Hat Quay. Must be deployed to the same OpenShift project as the quay-operator.
* [container-security-operator](container-security-operator) Installs Container Security Operator. This operator injects information on detected vulnerabilities to Kubernetes/OpenShift. Installation of this operator is *optional*. There's one instance of this operator installed per OpenShift cluster.

## Quick Start

Note that some of the following commands require *cluster-admin* role.

Create a new project called `quay-enterprise`:

```
$ oc new-project quay-enterprise
```

Install Quay operator to the `quay-enterprise` project:

```
$ oc apply --kustomize quay-operator/base
```

Refer to [Accessing Red Hat Red Hat Quay](https://access.redhat.com/solutions/3533201) to get the credentials to pull containers from the Quay.io registry. Save the credentials as `quay-instance/base/redhat-quay-pull-secret.json`.

Deploy a Red Hat Quay instance to the `quay-enterprise` project:
 
```
$ oc apply --kustomize quay-instance/overlays/development
```
In several minutes a development Quay installation should be up and running on your OpenShift cluster.

Optionally, install Container Security Operator:

```
$ oc apply --kustomize container-security-operator/base
```

## Deploying Red Hat Quay Using Argo CD

The provided kustomization can used as a base for a GitOps deployment of Red Hat Quay on OpenShift. Here are sample Argo CD application files. Remember to replace the `repoURL` with your own git repository URL:

```
# quay-operator-argocd.yaml
metadata:
  name: quay-operator
  namespace: argocd
spec:
  destination:
    namespace: quay-enterprise
    server: https://kubernetes.default.svc
  project: default
  source:
    path: quay-operator/base
    repoURL: https://noseka1:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@github.com/noseka1/quay-kustomization-priv.git
    targetRevision: HEAD
  syncPolicy:
    automated: {}
```

```
# quay-instance-argocd.yaml
metadata:
  name: quay-instance
  namespace: argocd
spec:
  destination:
    namespace: quay-enterprise
    server: https://kubernetes.default.svc
  project: default
  source:
    path: quay-instance/overlays/development
    repoURL: https://noseka1:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@github.com/noseka1/quay-kustomization-priv.git
    targetRevision: HEAD
  syncPolicy:
    automated: {}
```

```
# container-security-operator-argocd.yaml
metadata:
  name: container-security-operator
  namespace: argocd
spec:
  destination:
    namespace: openshift-operators
    server: https://kubernetes.default.svc
  project: default
  source:
    path: container-security-operator/base
    repoURL: https://noseka1:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@github.com/noseka1/quay-kustomization-priv.git
    targetRevision: HEAD
  syncPolicy:
    automated: {}
 ```

After your saved the above files to your local directory, you can start the GitOps deployment by issuing the following commands:

```
$ argocd app create --file quay-operator-argocd.yaml
$ argocd app create --file quay-instance-argocd.yaml
$ argocd app create --file container-security-operator-argocd.yaml
```
Here are some screenshots showing Red Hat Quay deployed using Argo CD:

![Quay Operator](docs/quay_operator_argocd.png "Quay Operator")
![Quay Instance](docs/quay_instance_argocd.png "Quay Instance")
![Container Security Operator](docs/container_security_operator_argocd.png "Container Security Operator")
