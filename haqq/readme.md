![Logo](https://raw.githubusercontent.com/Pa1amar/mainnets/main/haqq/haqq-logo2.png)
## haqq
| Attribute | Value |
|----------:|-------|
| Chain ID         | `haqq_11235-1` |
| RPC  | https://haqq-rpc.palamar.io:443 |
| API  | https://haqq-api.palamar.io:443 |
| GRPC | https://haqq-grpc.palamar.io:443 |
| EXPLORER | https://explorer.palamar.io/haqq/ |

## Install node
```bash
sudo apt update
sudo apt install make clang pkg-config libssl-dev build-essential git jq -y
```
#### Install go
```bash
cd $HOME
VERSION=1.20.4
wget -O go.tar.gz https://go.dev/dl/go$VERSION.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go.tar.gz && rm go.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
go version
```
#### Build binary
```bash
cd $HOME && rm -rf haqq
git clone https://github.com/haqq-network/haqq.git && cd haqq
git checkout v1.4.0
make build
sudo mv build/haqqd /usr/local/bin/
haqqd version
```
#### Init node and download genesis
```bash
haqqd init node --chain-id haqq_11235-1
wget -O $HOME/.haqqd/config/genesis.json https://raw.githubusercontent.com/Pa1amar/mainnets/main/haqq/genesis.json
haqqd tendermint unsafe-reset-all --home $HOME/.haqqd || haqqd unsafe-reset-all
wget -O $HOME/.haqqd/config/addrbook.json https://storage.palamar.io/mainnet/haqq/addrbook.json
```
## StateSync
```bash
SNAP_RPC="https://haqq-rpc.palamar.io:443"
PEER="6ad32961d595c715739f146f60d95830f8261bea@haqq-rpc.palamar.io:10456"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEER\"/" ~/.haqqd/config/config.toml
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.haqqd/config/config.toml

haqqd tendermint unsafe-reset-all --home $HOME/.haqqd || haqqd unsafe-reset-all
wget -O $HOME/.haqqd/config/addrbook.json https://storage.palamar.io/mainnet/haqq/addrbook.json
sudo systemctl restart haqqd 
journalctl -u haqqd -f --no-hostname -o cat
```
### Download addrbook.json (Updated every hour):
```bash
sudo systemctl stop haqqd
wget -O $HOME/.haqqd/config/addrbook.json https://storage.palamar.io/mainnet/haqq/addrbook.json
sudo systemctl start haqqd
```
