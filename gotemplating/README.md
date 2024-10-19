### Configure credentials

Create a file `credentials.txt`:

```yaml
[default]
aws_access_key_id = <aws_access_key>
aws_secret_access_key = <aws_secret_key>
```

```bash
kubectl create secret \
generic aws-secret \
-n crossplane-system \
--from-file=creds=./credentials.txt
```

```bash
k apply -f dependencies.yaml
```