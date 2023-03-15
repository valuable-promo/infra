# Headless ENVs

## APP_KEYS

Use `crypto.randomBytes(16).toString('base64')` to generate new keys (4x). Then

```bash
export APP_KEYS="key1,key2,key3,key4"
```

## API_TOKEN_SALT

Use `crypto.randomBytes(16).toString('base64')` to generate a new key. Then

```bash
export API_TOKEN_SALT="key"
```

## ADMIN_JWT_SECRET

Use `crypto.randomBytes(16).toString('base64')` to generate a new key. Then

```bash
export ADMIN_JWT_SECRET="key"
```

## JWT_SECRET

Use `crypto.randomBytes(16).toString('base64')` to generate a new key. Then

```bash
export JWT_SECRET="key"
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
    --from-literal=DATABASE_PASSWORD=$DATABASE_PASSWORD \
    --from-literal=SCALEWAY_ACCESS_SECRET=$SCALEWAY_ACCESS_SECRET \
    --from-literal=SMTP_PASSWORD=$SMTP_PASSWORD \
    -o yaml \
    | kubeseal --format=yaml > $ENV.env.sealed-secret.yaml
```

```bash
kubectl apply -f $ENV.env.sealed-secret.yaml
```
