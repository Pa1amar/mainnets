## Aura
| Attribute | Value |
|----------:|-------|
| Chain ID         | `umee-1` |
| RPC  | https://aura-rpc.palamar.io:443 |
| API  | https://aura-api.palamar.io:443 |
| GRPC | https://aura-grpc.palamar.io:443 |
| EXPLORER | https://explorer.palamar.io/aura/ |
## Install node
```bash
sudo apt update
sudo apt install make clang pkg-config libssl-dev build-essential git -y
```
#### Install go
```bash
cd $HOME
VERSION=1.19.7
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
cd $HOME && rm -rf aura
git clone https://github.com/aura-nw/aura.git && cd aura
git checkout aura_v0.4.4
make build
sudo mv build/aurad /usr/local/bin/
aurad version
```
#### Init node and download genesis
```bash
aurad init node --chain-id xstaxy-1
wget -O $HOME/.aura/config/genesis.json https://raw.githubusercontent.com/aura-nw/mainnet-artifacts/main/xstaxy-1/genesis.json
aurad tendermint unsafe-reset-all --home $HOME/.aura || aurad unsafe-reset-all
wget -O $HOME/.aura/config/addrbook.json https://storage.palamar.io/mainnet/aura/addrbook.json
```
#### Config node
```bash
sed -i 's/^minimum-gas-prices *=.*/minimum-gas-prices = "0.001uaura"/' $HOME/.aura/config/app.toml
```
#### Create service and start node
```bash
echo "[Unit]
Description=Umee Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/usr/local/bin/aurad start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/aurad.service
sudo mv $HOME/aurad.service /etc/systemd/system
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```
```bash
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable aurad
```
## StateSync
```bash
SNAP_RPC="https://aura-rpc.palamar.io:443"
PEER="27a9ed6dabd615c5bb7f7b10a0fd505b4670c301@aura-rpc.palamar.io:10456"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEER\"/" ~/.aura/config/config.toml
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.aura/config/config.toml

aurad tendermint unsafe-reset-all --home $HOME/.aura || aurad unsafe-reset-all
sudo systemctl restart aurad 
journalctl -u aurad -f --no-hostname -o cat
```
### Download addrbook.json:
```bash
sudo systemctl stop aurad
wget -O $HOME/.aura/config/addrbook.json https://storage.palamar.io/mainnet/umee/addrbook.json
sudo systemctl start aurad
```
