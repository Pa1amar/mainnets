## Install node:
```bash
wget -O  penumbra.sh https://raw.githubusercontent.com/Pa1amar/testnets/main/penumbra/penumbra.sh && sudo chmod +x penumbra.sh && /bin/bash penumbra.sh
```
check penumbra node logs
```bash
sudo journalctl -u penumbra -f -o cat
```
check tendermint node logs
```bash
sudo journalctl -u cometbft -f -o cat
```
