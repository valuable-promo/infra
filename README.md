# Infra

We have `staging` and `prod` namespaces. Cloud enginner provided the following resources for each namespace:

- PostgreSQL username and database
- SMTP credentials
- Object storage credentials
- All the secrets (sealed)

Happy coding!

## dbbackup

### AWS S3 Settings

Create a bucket with the following lifecycle rules:

- Expire current versions of objects (x days)
- Permanently delete noncurrent versions of objects (x days)

### AWS IAM Settings

Create a user with only write permission to s3 bucket and get the access key and secret key:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject"],
      "Resource": ["arn:aws:s3:::YOUR_BUCKET", "arn:aws:s3:::YOUR_BUCKET/*"]
    }
  ]
}
```

### Sealed Secrets

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

## frontend

### Generate a random token for revalidation

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

## gitops

Generate a new GPG key pair:

```bash
gpg --full-generate-key
```

Follow the prompts to specify the key type, size, and expiration. You'll also need to provide a user ID and a passphrase.

List the generated keys:

```bash
gpg --list-secret-keys --keyid-format LONG
```

Take note of the key ID which appears after the rsa part, e.g., ABCDEF1234567890.

Export the GPG private key:

```bash
gpg --export-secret-keys --armor KEY_ID > git.asc
```

Export the GPG public key and copy to github user's GPG keys (powerkernelbot):

```bash
gpg --export --armor KEY_ID
```

Create sealed secret:

```bash
export NAMESPACE='toBeDefined'
kubectl create secret generic --dry-run=client \
    fluxcd-signing-key \
    --namespace=$NAMESPACE \
    --from-file=git.asc \
    -o yaml \
    | kubeseal --format=yaml > fluxcd-signing-key.sealed-secret.yaml
```

Flux notifcation:

```bash
export SLACK_WEBHOOK=https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK
kubectl create secret generic --dry-run=client slack-url --namespace=$NAMESPACE \
--from-literal=address=$SLACK_WEBHOOK \
-o yaml | kubeseal --format=yaml > slack-url.sealed-secret.yaml
```

## headless

### Secrets

```bash
export APP_KEYS="$(openssl rand -base64 16),$(openssl rand -base64 16),$(openssl rand -base64 16),$(openssl rand -base64 16)"
export API_TOKEN_SALT="$(openssl rand -base64 16)"
export ADMIN_JWT_SECRET="$(openssl rand -base64 16)"
export JWT_SECRET="$(openssl rand -base64 16)"
export TRANSFER_TOKEN_SALT="$(openssl rand -base64 16)"
```

### SCALEWAY_ACCESS_SECRET

Provided by the cloud engineer.

```bash
export SCALEWAY_ACCESS_SECRET="secret"
```

### SMTP_PASSWORD

Provided by the cloud engineer.

```bash
export SMTP_PASSWORD="password"
```

### Create sealed secrets

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
    --from-literal=SCALEWAY_ACCESS_SECRET=$SCALEWAY_ACCESS_SECRET \
    --from-literal=SMTP_PASSWORD=$SMTP_PASSWORD \
    -o yaml \
    | kubeseal --format=yaml > sealed-secret.yaml
```

## revalidator

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
