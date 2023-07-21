# Revalidator

## Secrets

```bash
export NS="toBeProvided"
export REVALIDATE_TOKEN="toBeProvided"
kubectl create secret generic --dry-run=client \
    revalidator-env \
    --namespace=$NS \
    --from-literal=REVALIDATE_TOKEN=$REVALIDATE_TOKEN \
    -o yaml \
    | kubeseal --format=yaml > sealed-secret.yaml
```
