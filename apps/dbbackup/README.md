# dbbackup

## Sealed Secrets

```bash
export NAMESPACE='toBeDefined'
export AWS_SECRET_ACCESS_KEY='toBeDefined'
export PGPASSWORD='toBeDefined'

kubectl create secret generic --dry-run=client \
    dbbackup-env \
    --namespace=$NAMESPACE \
    --from-literal=AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
    --from-literal=PGPASSWORD=$PGPASSWORD \
    -o yaml \
    | kubeseal --format=yaml > sealed-secret.yaml
```
