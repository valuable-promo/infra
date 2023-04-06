# Headless ENVs

## Secrets

```bash
export APP_KEYS="$(openssl rand -base64 16),$(openssl rand -base64 16),$(openssl rand -base64 16),$(openssl rand -base64 16)"
export API_TOKEN_SALT="$(openssl rand -base64 16)"
export ADMIN_JWT_SECRET="$(openssl rand -base64 16)"
export JWT_SECRET="$(openssl rand -base64 16)"
export TRANSFER_TOKEN_SALT="$(openssl rand -base64 16)"
```

## DATABASE_PASSWORD

Provided by the cloud engineer.

```bash
export DATABASE_PASSWORD="password"
```

## SCALEWAY_ACCESS_SECRET

Provided by the cloud engineer.

```bash
export SCALEWAY_ACCESS_SECRET="secret"
```

## SMTP_PASSWORD

Provided by the cloud engineer.

```bash
export SMTP_PASSWORD="password"
```

## Create sealed secrets

```bash
export NS="valuable-promo"
export ENV="staging"
```

```bash
kubectl create secret generic --dry-run=client \
    headless-env \
    --namespace=$NS-$ENV \
    --from-literal=APP_KEYS=$APP_KEYS \
    --from-literal=API_TOKEN_SALT=$API_TOKEN_SALT \
    --from-literal=ADMIN_JWT_SECRET=$ADMIN_JWT_SECRET \
    --from-literal=JWT_SECRET=$JWT_SECRET \
    --from-literal=TRANSFER_TOKEN_SALT=$TRANSFER_TOKEN_SALT \
    --from-literal=DATABASE_PASSWORD=$DATABASE_PASSWORD \
    --from-literal=SCALEWAY_ACCESS_SECRET=$SCALEWAY_ACCESS_SECRET \
    --from-literal=SMTP_PASSWORD=$SMTP_PASSWORD \
    -o yaml \
    | kubeseal --format=yaml > $ENV/env.sealed-secret.yaml
```

```bash
kubectl apply -f $ENV/
```
