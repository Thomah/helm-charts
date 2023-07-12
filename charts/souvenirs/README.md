# Souvenirs

[Souvenirs](https://github.com/souvenirs/) is an open sourced solution for collaborative project management.

## Introduction

This chart bootstraps an instance of 'Souvenirs'.

## Prerequisites

- Kubernetes 1.14+

## Installing the chart

To install the chart:

```bash
$ helm repo add souvenirs https://souvenirs.github.io/helm-charts
$ helm install souvenirs/souvenirs
```

## Uninstalling the chart

To uninstall/delete the deployment:

```bash
$ helm list
NAME           REVISION    UPDATED                     STATUS      CHART                          NAMESPACE
kindly-newt    1           Tue May  4 11:08:43 2021    DEPLOYED    souvenirs-1.0.0    default
$ helm delete kindly-newt
```

### Using external PostgreSQL database

1. Create the following ExternalName Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db-postgres
spec:
  externalName: db.example.com
  type: ExternalName
```

2. Add the following Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-souvenirs
data:
  username: <Username in Base64>
  password: <Password in Base64>
```

3. Set the following values of the chart:

```yaml
db:
  url: jdbc:postgresql://db-souvenirs:5432/souvenirs
  existingSecret:
    name: db-souvenirs
    usernameKey: username
    passwordKey: password

postgresql:
  enabled: false
```
