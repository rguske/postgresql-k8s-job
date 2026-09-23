# Postgresql-k8s-Job

This project contains a Kubernetes Job that initializes a PostgreSQL database for virtual machine data. The Job connects to PostgreSQL using credentials stored in a Kubernetes Secret, creates the `vmdb` database when it does not already exist, and creates the `virtual_machines` table if necessary.

The table contains columns for the virtual machine type, ID, kind, name, namespace, timestamp, instance type, CPU cores, CPU sockets, memory, storage class, and network. The initialization is idempotent, so existing databases and tables are preserved.

Create the secret:

```shell
oc -n postgresql create secret generic postgresql-job-secret \
  --from-literal=DB_HOST='postgres-svc.postgresql.svc.cluster.local' \
  --from-literal=DB_USER=postgres \
  --from-literal=POSTGRES_PASSWORD='redhat'
  # defined here: https://github.com/rguske/postgresql-statefulset-example/blob/main/base/secret.yaml
```

Namespace of your choice:

```shell
export NAMESPACE='postgresql'
```

Run the job:

```bash
kubectl -n ${NAMESPACE} apply -k base/kustomization.yaml
```
