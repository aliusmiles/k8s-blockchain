# k8s-blockchain

K8S Manifests for blockchain deployment

## Prereqs

* K8S cluster: kind/k3s/eks/etc

### Kind cluster

```sh
cat <<EOF | kind create cluster --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
EOF
```

### Ingress-nginx

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Besu

### generate config/secrets

```sh
mkdir temp
cd temp
npx quorum-genesis-tool
```

### deploy

```sh
kubectl create ns besu
kubectl apply --recursive -f besu/config
kubectl apply --recursive -f besu/validator
```

## Geth

### generate config/secrets

requires geth installed

[genesis.json](https://geth.ethereum.org/docs/interface/private-network)

To create the initial extradata for your network, collect the signer addresses and encode extradata as the concatenation of 32 zero bytes, all signer addresses, and 65 further zero bytes.

```sh
mkdir temp
cd temp
for i in {0..2}; do bootnode -genkey node-$i -writeaddress > node-$i.pub; done
for i in {0..3}; do touch account.password && geth account new --password account.password --keystore account-$i; done
```

### deploy

```sh
kubectl create ns geth
kubectl apply --recursive -f . geth
```
