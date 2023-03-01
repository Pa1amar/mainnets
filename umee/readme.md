## Umee
| Attribute | Value |
|----------:|-------|
| Chain ID         | `umee-1` |
| RPC  | https://rpc.umee-1.palamar.io:443 |
| API  | https://api.umee-1.palamar.io:443 |
| GRPC | https://grpc.umee-1.palamar.io:443 |
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
