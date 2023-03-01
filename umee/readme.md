## Umee
| Attribute | Value |
|----------:|-------|
| Chain ID         | `umee-1` |
| RPC  | https://rpc.umee-1.palamar.io:443 |
| API  | https://api.umee-1.palamar.io:443 |
| GRPC | https://grpc.umee-1.palamar.io:443 |
## Install node
```bash
sudo apt update
sudo apt install make clang pkg-config libssl-dev build-essential git -y
```
#### Install go
```bash
cd $HOME
VERSION=1.19.6
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
cd $HOME
git clone https://github.com/umee-network/umee.git && cd umee
git checkout v4.1.0
make build
sudo mv /build/umeed /usr/local/bin/
umeed version
```
### Create service and start node
```bash
echo "[Unit]
Description=Umee Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/usr/local/bin/umeed start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/umeed.service
sudo mv $HOME/umeed.service /etc/systemd/system
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```
```bash
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable umeed
sudo systemctl restart umeed
```

## StateSync
```bash
sudo systemctl stop umeed
SNAP_RPC="https://rpc.umee-1.palamar.io:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.umee/config/config.toml

umeed tendermint unsafe-reset-all --home $HOME/.umee || umeed unsafe-reset-all
sudo systemctl restart umeed 
journalctl -u umeed -f --no-hostname -o cat
```
## Download addrbook.json:
```bash
sudo systemctl stop umeed
wget -O $HOME/.umee/config/addrbook.json https://storage.palamar.io/mainnet/umee/addrbook.json
sudo systemctl start umeed
```
