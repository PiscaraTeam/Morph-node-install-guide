# Morph-node-install-guide
This guide will help you easily install a node for the Morph project and become a full-fledged member of testnet

# Install 
## Clone Morph
```
mkdir -p ~/.morph 
cd ~/.morph
git clone https://github.com/morph-l2/morph.git\
git checkout v0.1.0-beta
```

## Build Geth
```
make nccc_geth
```

## Build Node
```
cd ~/.morph/morph/node 
make build
```
## Sync from genesis block
```
# Download and unzip

cd ~/.morph
wget https://raw.githubusercontent.com/morph-l2/config-template/main/holesky/data.zip
unzip data.zip
```

```
# Create a shared secret with node

cd ~/.morph
openssl rand -hex 32 > jwt-secret.txt
```

## Geth start
```
./morph/go-ethereum/build/bin/geth --morph-holesky \
    --datadir "./geth-data" \
    --http --http.api=web3,debug,eth,txpool,net,engine \
    --authrpc.addr localhost \
    --authrpc.vhosts="localhost" \
    --authrpc.port 8551 \
    --authrpc.jwtsecret=./jwt-secret.txt \
    --miner.gasprice="100000000" \
    --log.filename=./geth.log
```
## Node start
```
./morph/node/build/bin/morphnode --home ./node-data \
     --l2.jwt-secret ./jwt-secret.txt \
     --l2.eth http://localhost:8545 \
     --l2.engine http://localhost:8551 \
     --log.filename ./node.log
```
