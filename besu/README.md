# besu

besu config


```sh
mkdir temp
cd temp
for i in {0..3}; do bes -genkey node-$i -writeaddress > node-$i.pub; done
besu operator generate-blockchain-config --config-file=config.json --to=myNetworkFiles

```
