# dbbackup

## AWS S3 Settings

Create a bucket with the following lifecycle rules:

- Expire current versions of objects (x days)
- Permanently delete noncurrent versions of objects (x days)

## AWS IAM Settings

Create a user with only write permission to s3 bucket and get the access key and secret key.:

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
