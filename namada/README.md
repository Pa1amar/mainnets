![Logo](https://raw.githubusercontent.com/Pa1amar/mainnets/refs/heads/main/namada/logo.png)
## Namada
| Attribute | Value |
|----------:|-------|
| Chain ID         | `namada.5f5de2dd1b88cba30586420` |
| RPC (tx_index: on)  | https://namada-rpc.palamar.io:443 |
| MASP  | https://namada-masp.palamar.io:443 |
| INDEXER | https://namada-indexer.palamar.io:443 |
| INTERFACE | https://namada-interface.palamar.io |
| DATA | https://storage.palamar.io/mainnet/namada |

### Download snapshot
```bash
sudo systemctl stop namadad
cp $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/cometbft/data/priv_validator_state.json $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/priv_validator_state.json.backup
rm -rf $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/cometbft/data
rm -rf $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/db
curl https://storage.palamar.io/mainnet/namada/snapshot_latest.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/
mv $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/priv_validator_state.json.backup $HOME/.local/share/namada/namada.5f5de2dd1b88cba30586420/cometbft/data/priv_validator_state.json
sudo systemctl restart namadad
```
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
tcp://6b469eb00f21d6ebe344c951f599e2012f70d4e9@5.194.81.121:19904,tcp://aebbde037a88ac347c565fef002f125b6e9e0c83@51.178.74.93:26656,tcp://68ede0c21b03bfeb3ace802eaafbdd2b55d5c215@161.35.198.105:38656,tcp://c1410f11db5522e176e69100816ea5bbe8c99e36@188.214.130.102:26670,tcp://219c4c2475048dbaa9e01d20ebd82b913958b4d8@72.46.84.33:16656,tcp://a8187523daabbc053ec992cde9975f65a085da25@46.4.29.231:5000,tcp://0edc3530905568e7963c1c39c78061a1a1ed44af@79.127.240.32:26656,tcp://75e35d1fdad3e243ff828803b0b371ae69a249e2@62.3.101.89:26656,tcp://91bb5973a676bb20f095d8f6d18433413cb5d78f@141.95.11.197:26656,tcp://3eb52b18e1ccfd787d558ff8a1444b39ca57575e@173.231.17.98:26656,tcp://478de66fe39df43a60f5850e5b99da4edd14de85@212.51.129.72:26706,tcp://74184876d3b02a7d622f177779a416aa66964bdd@51.91.105.170:26656,tcp://5c479b8d9969bb901897ebed40fc197d507f007c@144.91.119.1:26656,tcp://ee080652cd61f73e8552ef6a91e9e622e7f8d2b0@62.3.101.91:26656,tcp://593109ec6db7a1b15cae99cc85cc2b5cb2ca3f67@51.81.34.21:26656,tcp://05309c2cce2d163027a47c662066907e89cd6b99@104.251.123.123:26656,tcp://96f7945f9470faacce66888d798bf1f131913b6c@62.210.95.44:26656,tcp://c4deb6863d50bcdd9d20b02303d010090908d6d2@192.64.82.62:26656
```
