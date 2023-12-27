# Database

```bash
kubectl create secret generic db-secret --from-literal=username=app --from-literal=password=$(openssl rand -base64 32) -n valuable-promo-staging --dry-run=client -o yaml | kubeseal --format yaml > sealed-secret.yaml
```

Restore database

```bash
psql -U app -h headless-pooler.valuable-promo-staging -d app -W -f valuable_promo_staging_20231225000005.sql
```
