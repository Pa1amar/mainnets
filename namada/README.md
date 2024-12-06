![Logo](https://raw.githubusercontent.com/Pa1amar/mainnets/refs/heads/main/namada/logo.png)
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
wget -O $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/cometbft/config/addrbook.json https://storage.palamar.io/mainnet/namada/addrbook.json
sudo systemctl start namadad
```
### Peer
```bash
tcp://d86c6c8bc56781fd93794ca7af6f0c0e90e34584@namada-peer.palamar.io:16656
```














### Live Peers
```
tcp://e440b899fadb26e41745dc741d5f75b8f8aa251a@65.109.30.26:14656,tcp://d83cd082b8973644e381fad9421ca29fb50fe059@65.108.73.189:20400,tcp://4fc1398cb721afd3e73a00281b13d5fec0ce7566@138.201.221.23:26656,tcp://f599bec873183d371ae22f89195d3ced22dda2f3@46.4.29.231:5000,tcp://86238829d64fe2fa5b4337ca90926f9ec56445f2@193.35.57.185:36656,tcp://04affb50117ef548cbf7d1ddb1e6416dec0645ae@65.108.75.179:14656,tcp://20d302d5cf8e85ef8c9f8c38d0c5e87d5f3620a6@34.13.128.48:26656,tcp://04f840d09db8d7c409cecb963f37485200904423@93.159.130.40:28656,tcp://509f1e843cf881650a4151aa804ddd7a7188e88f@195.201.197.246:32656,tcp://35bea1f9d7a2f34ac093ae361c6876b328d8cf20@172.161.145.12:26656,tcp://1cb0c9813db48396b31976443a1cd88b73e0fb05@95.216.78.215:26656,tcp://e461529f0cfc2520dbad23d402906924fef602f9@65.109.26.242:26656
```
