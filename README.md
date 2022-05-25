# k8s-blockchain

K8S Manifests for blockchain deployment

## Prereqs

* K8S cluster: kind/k3s/eks/etc

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
