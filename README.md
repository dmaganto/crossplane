# Crossplane
Repository for crossplane

## Install crossplane
```bash
kubectl create namespace crossplane-system
helm repo add crossplane-stable <https://charts.crossplane.io/stable>
helm repo update

helm install crossplane --namespace crossplane-system crossplane-stable/crossplane
````
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
### Install provider AWS

You have two options, [oficial crossplane](https://marketplace.upbound.io/providers/upbound/provider-aws/v0.18.0) or [aws contrib](https://marketplace.upbound.io/providers/crossplane-contrib/provider-aws/v0.36.1)
```bash
cat <<EOF | kubectl apply -f -
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-aws
spec:
  package: xpkg.upbound.io/crossplane-contrib/provider-aws:v0.33.0
EOF
```

```bash 
cat <<EOF | kubectl apply -f -
apiVersion: aws.crossplane.io/v1beta1
kind: ProviderConfig
metadata:
  name: provider-config-aws
spec:
  credentials:
    secretRef:
      namespace: crossplane-system
      name: aws-secret
      key: creds
    source: Secret
  endpoint:
    url:
      dynamic:
        host: amazonaws.com
        protocol: https
      type: Dynamic
EOF
```


