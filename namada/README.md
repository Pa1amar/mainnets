![Logo](https://github.com/Pa1amar/mainnets/blob/main/namada/y4D30_VO_400x400.jpg)
## Namada
| Attribute | Value |
|----------:|-------|
| Chain ID         | `namada.5f5de2dd1b88cba30586420` |
| RPC  | https://namada-rpc.palamar.io:443 |
| MASP  | https://namada-masp.palamar.io:443 |
| INDEXER | https://namada-indexer.palamar.io:443 |
| INTERFACE | https://namada-interface.palamar.io |

### Download addrbook.json (Updated every hour):
```bash
sudo systemctl stop namadad
wget -O $HOME//.local/share/namada/namada.5f5de2dd1b88cba30586420/cometbft/config/addrbook.json https://storage.palamar.io/mainnet/namada/addrbook.json
sudo systemctl start namadad
```
