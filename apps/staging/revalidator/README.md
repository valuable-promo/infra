# Revalidator

## Secrets

```bash
export NAMESPACE="toBeProvided"
export REVALIDATE_TOKEN="toBeProvided"
kubectl create secret generic --dry-run=client \
    revalidator-env \
    --namespace=$NAMESPACE \
    --from-literal=REVALIDATE_TOKEN=$REVALIDATE_TOKEN \
    -o yaml \
    | kubeseal --format=yaml > sealed-secret.yaml
```
