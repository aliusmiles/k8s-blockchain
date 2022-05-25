# k8s-blockchain

K8S Manifests for blockchain deployment

## Prereqs

* K8S cluster: kind/k3s/eks/etc

## Besu

```sh
kubectl create ns besu
kubectl apply --recursive -f besu/config
kubectl apply --recursive -f besu/validator
```
