# Frontend

## Generate a random token for revalidation

```bash
export REVALIDATE_TOKEN="$(openssl rand -base64 32)"
```

```bash
export NAMESPACE="toBeProvided"
kubectl create secret generic --dry-run=client \
    frontend-env \
    --namespace=$NAMESPACE \
    --from-literal=REVALIDATE_TOKEN=$REVALIDATE_TOKEN \
    -o yaml \
    | kubeseal --format=yaml > sealed-secret.yaml
```
