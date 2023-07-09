# FluxCD Signing Key

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
    | kubeseal --format=yaml > sealed-secret.yaml
```
