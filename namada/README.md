![Logo](https://raw.githubusercontent.com/Pa1amar/mainnets/refs/heads/main/namada/logo.png)
## Namada
| Attribute | Value |
|----------:|-------|
| Chain ID         | `namada.5f5de2dd1b88cba30586420` |
| RPC  | https://namada-rpc.palamar.io:443 |
| MASP  | https://namada-masp.palamar.io:443 |
| INDEXER | https://namada-indexer.palamar.io:443 |
| INTERFACE | https://namada-interface.palamar.io |

### Peer
```bash
tcp://d86c6c8bc56781fd93794ca7af6f0c0e90e34584@namada-peer.palamar.io:16656
```

### Download addrbook.json (Updated every hour):
```bash
sudo systemctl stop namadad
wget -O $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/cometbft/config/addrbook.json https://storage.palamar.io/mainnet/namada/addrbook.json
sudo systemctl start namadad
```
