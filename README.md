# Deploy Airflow on OpenShift

## Prepare namespace

```shell
oc new-project airflow
oc new-app -e POSTGRESQL_ADMIN_PASSWORD=airflow --name airflow-psql postgresql:13-el9
```

## Deploy Airflow

```shell
helm upgrade --install airflow apache-airflow/airflow --namespace airflow --values values.yml
```
## Expose Airflow

```shell
oc expose svc/airflow-webserver
oc patch route airflow-webserver --patch '{"spec":{"tls":{"termination":"edge","insecureEdgeTerminationPolicy":"Redirect"}}}'
```
