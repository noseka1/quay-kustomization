# Quay Operator

[Quay Operator](https://github.com/redhat-cop/quay-operator)

```
$ oc new-project quay-enterprise
```

```
$ oc apply --kustomize overlays/development
```

## Configuration

Edit `base/redhat-quay-pull-secret.json` and insert your Red Hat Quay pull secret. This secret allows one to pull images from `quay.io/redhat`. You can obtain the Red Hat Quay secret by following the instructions at [Accessing Red Hat Quay (formerly Quay Enterprise) without a CoreOS login](https://access.redhat.com/solutions/3533201), section "JSON config.json for Red Hat Quay v3". The secret is a JSON snippet that can be directly inserted into the `.dockerconfigjson` field of a Kubernetes Secret object.
