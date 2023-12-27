# Database

```bash
kubectl create secret generic db-secret --from-literal=username=app --from-literal=password=$(openssl rand -base64 32) -n valuable-promo-prod --dry-run=client -o yaml | kubeseal --format yaml > sealed-secret.yaml
```

Restore database

```bash
psql -U app -h headless-pooler.valuable-promo-prod -d app -W -f valuable_promo_prod_20231225010103.sql
```
