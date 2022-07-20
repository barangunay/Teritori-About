# Teritori-About
### Greetings, today we will be joining the Teritori Network's testnet on Cosmos, enjoy reading.
# system requirements

```
2 CPU
2 RAM
80 GB SSD
```

# We set up our machine
```
apt update && apt upgrade -y 
```

```
apt install build-essential git curl gcc make jq -y
```
# Go 
```
wget -c https://go.dev/dl/go1.18.3.linux-amd64.tar.gz && rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.3.linux-amd64.tar.gz && rm -rf go1.18.3.linux-amd64.tar.gz
```

```
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
```
# We create the chain
```
git clone https://github.com/TERITORI/teritori-chain && cd teritori-chain && git checkout teritori-testnet-v2 && make install
```
# Let's start the network
```
teritorid init <VALİDATOR Name> --chain-id teritori-testnet-v2
```
# Configuration file:
```
sed -i.bak 's/persistent_peers =.*/persistent_peers = "0b42fd287d3bb0a20230e30d54b4b8facc412c53@176.9.149.15:26656,2371b28f366a61637ac76c2577264f79f0965447@176.9.19.162:26656,2f394edda96be07bf92b0b503d8be13d1b9cc39f@5.9.40.222:26656"/' $HOME/.teritorid/config/config.toml
```
# Let's download the Genesis file:
```
wget -O ~/.teritorid/config/genesis.json https://raw.githubusercontent.com/TERITORI/teritori-chain/main/testnet/teritori-testnet-v2/genesis.json
```
# Service file
```
tee <<EOF >/dev/null /etc/systemd/system/teritorid.service
[Unit]
Description=Teritori Cosmos daemon
After=network-online.target

[Service]
User=root
ExecStart=/root/go/bin/teritorid start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```
```
systemctl enable teritorid
systemctl daemon-reload
systemctl restart teritorid
```
# Now we go inside the service file:
```
sudo nano /etc/systemd/system/teritorid.service
```
```
sudo systemctl stop teritorid
rm $HOME/.teritorid/config/addrbook.json
wget -O $HOME/.teritorid/config/addrbook.json https://raw.githubusercontent.com/StakeTake/guidecosmos/main/teritori/teritori-testnet-v2/addrbook.json
sudo systemctl restart teritorid
```
