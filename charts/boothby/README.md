# Boothby

## Using external MySQL / MariaDB database

1. Create a yaml file `db-postgres.yaml` which will bind a local kubernetes host `db-postgres` to the external hostname of the database

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db-postgres
spec:
  externalName: db.example.com
  type: ExternalName
```

2. Create a yaml file `db-boothby.yaml` with a secret containing credentials to access database

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-boothby
data:
  PGUSER: <Username in Base64>
  PGPASSWORD: <Password in Base64>
```

3. Upload the two files

```shell
$ kubectl apply -f db-postgres.yaml
$ kubectl apply -f db-boothby.yaml
```

4. Set the following values of the chart:

```yaml
db:
  host: db-postgres
  name: boothby
  existingSecret:
    name: db-boothby

postgresql:
  enabled: false
```
