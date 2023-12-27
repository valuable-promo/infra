# Database

```bash
kubectl create secret generic org-db-cluster-app-secret --from-literal=username=app --from-literal=password=$(openssl rand -base64 32) -n mirasys --dry-run=client -o yaml | kubeseal --format yaml > sealed-secret.yaml
```
