![Logo](https://raw.githubusercontent.com/Pa1amar/mainnets/main/haqq/haqq-logo2.png)
## haqq
| Attribute | Value |
|----------:|-------|
| Chain ID         | `haqq_11235-1` |
| RPC  | https://haqq-rpc.palamar.io:443 |
| API  | https://haqq-api.palamar.io:443 |
| GRPC | https://haqq-grpc.palamar.io:443 |
| EXPLORER | https://explorer.palamar.io/haqq/ |

## StateSync
```bash
SNAP_RPC="https://haqq-rpc.palamar.io:443"
PEER="6ad32961d595c715739f146f60d95830f8261bea@haqq-rpc.palamar.io:10456"
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$PEER\"/" ~/.haqq/config/config.toml
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.haqq/config/config.toml

haqqd tendermint unsafe-reset-all --home $HOME/.haqq || haqqd unsafe-reset-all
sudo systemctl restart haqqd 
journalctl -u haqqd -f --no-hostname -o cat
```
### Download addrbook.json (Updated every hour):
```bash
sudo systemctl stop haqqd
wget -O $HOME/.haqq/config/addrbook.json https://storage.palamar.io/mainnet/haqq/addrbook.json
sudo systemctl start haqqd
```
