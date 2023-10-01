<p align="center">
 <img src="https://i.postimg.cc/QCQNc0h1/Centrifuge.jpg"/></a>
</p>

# In this guide we will install a Centrifuge Testnet RPC node.

## 1. Requirements.

#### Official 
- 4 CPU or more
- 16 GB RAM
- 500 GB SSD
  
#### My Recommendations
- I recommend Dedicated Ryzen 5 Server on [webtropia](https://bit.ly/45KaUj4)
- I recommend for convenience the SSH terminal - [MobaXTerm](https://mobaxterm.mobatek.net/download.html).

## 2. Server preparation.
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install make clang pkg-config libssl-dev libclang-dev build-essential git curl ntp jq llvm tmux htop screen unzip cmake -y
```
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
```

## 3. Install a Centrifuge node
```
git clone https://github.com/centrifuge/centrifuge-chain
cd centrifuge-chain
latestTag=$(curl -s https://api.github.com/repos/centrifuge/centrifuge-chain/releases/latest | grep '.tag_name'|cut -d\" -f4)
echo $latestTag
git checkout $latestTag
cargo build --release
sudo chmod +x target/release/centrifuge-chain
sudo mkdir data
cd ~
```

## 4. Create a systemd file
Take care about `--base-path` flag. You should input your `data` folder location. In my case this is `/home/centrifuge/centrifuge-chain/data`
```
sudo tee <<EOF >/dev/null /etc/systemd/system/centrifuge.service
[Unit]
Description="Centrifuge Testnet systemd service"
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=10
User=$USER
SyslogIdentifier=centrifuge_testnet
SyslogFacility=local7
KillSignal=SIGHUP
ExecStart=/home/centrifuge/centrifuge-chain/target/release/centrifuge-chain \
    --name=YOUR_NODE_NAME \
    --port=30333 \
    --rpc-port=9933 \
    --ws-port=9944 \
    --unsafe-ws-external \
    --unsafe-rpc-external \
    --rpc-cors=all \
    --rpc-methods=unsafe \
    --pruning=archive \
    --chain=algol \
    --parachain-id=2088 \
    --base-path=/home/centrifuge/centrifuge-chain/data \
    --log=main,info \
    --execution=wasm \
    --wasm-execution=compiled \
    --ws-max-connections=5000 \
    --bootnodes=/dnsaddr/bootstrap.algol.k-f.dev/p2p/12D3KooWQt8qAmXzgdNRuVKhW1NAdfowhoGC3apycsBYhvehp1mA \
    --bootnodes=/dns4/node-7107773973489594368-0.p2p.onfinality.io/tcp/28330/ws/p2p/12D3KooWHAsVwkowLLnwd9ZRAmNFBdsvFzvB9XS8FhEufhvuvec3 \
    --bootnodes=/ip4/34.141.22.46/tcp/30333/p2p/12D3KooWPdGhxLeQad8qfxr7cPrYGMW7sJhXL4rfjZMMJpWZ1nHd \
    --bootnodes=/ip4/34.141.85.156/tcp/30333/p2p/12D3KooWH8B1nrkNRvKPTuhaaXy244PZoptRzfXPQqVai3bRq4TS \
    --bootnodes=/ip4/34.141.48.250/tcp/30333/p2p/12D3KooWHsA69Fb1HkdWrwrxY3cSga3wWyHnxuk5GeYtCh59XsWB \
    -- \
    --no-telemetry \
    --execution=wasm \
    --wasm-execution=compiled \
    --chain=/home/centrifuge/centrifuge-chain/res/rococo-local.json \
    --bootnodes=/dnsaddr/bootstrap-relay.algol.k-f.dev/p2p/12D3KooWABjwDpbaapnTwei9ZhSLMHAWij5oZE4CsbsN8K8q4mYb \
    --bootnodes=/dnsaddr/bootstrap-relay-2.algol.k-f.dev/p2p/12D3KooWRQqyBqsFw8gw2oqZHaiPLyw2jgCyz5jwRMmg3aWr3jP5

[Install]
WantedBy=multi-user.target
EOF
```
                                            
## 5. Start synchronization
```
sudo systemctl daemon-reload
sudo systemctl start centrifuge
sudo systemctl enable centrifuge
sudo journalctl -u centrifuge -f -n 100 -o cat
```

- You can check yourself in [Centrifuge Polkadot Telemetry](https://telemetry.polkadot.io/#list/0xb3db41421702df9a7fcac62b53ffeac85f7853cc4e689e0b93aeb3db18c09d82) by your name
- Also you can check your synch by comand bellow:
```
curl -H "Content-Type: application/json" -d '{{"id":1, "jsonrpc":"2.0", "method": "eth_syncing", "params":[]}}' localhost:9933
```
The output `{"jsonrpc":"2.0","result":false,"id":1}` means that you are succesfully synced.

## 6. Deleting a node
```
sudo systemctl stop centrufuge
sudo rm -rf centrifuge-chain
sudo rm -rf /etc/systemd/system/centrufuge.service
```

## 7. Upgrade your node
```
cd centrifuge-chain
latestTag=$(curl -s https://api.github.com/repos/centrifuge/centrifuge-chain/releases/latest | grep '.tag_name'|cut -d\" -f4)
echo $latestTag
git checkout $latestTag
cargo build --release
sudo chmod +x target/release/centrifuge-chain
sudo ./target/release/centrifuge-chain --version
sudo systemctl restart centrifuge
```
Then check your logs
```
sudo journalctl -u centrifuge -f -n 100 -o cat
```

## 8. Your RPC URL
- `http://YOUR_IP_ADDRESS:9933` 

#
👉[Webtropia](https://bit.ly/45KaUj4) Only Dedicated Server.

👉[SSH terminal MobaxTerm](https://mobaxterm.mobatek.net/download.html)

👉[Discord](https://discord.gg/r5SSnqXyQG)

👉[WebSite](https://centrifuge.io/)

👉[Official Github](https://github.com/centrifuge/centrifuge-chain)

👉[Official guide](https://docs.centrifuge.io/)

👉[Centrifuge Explorer](https://telemetry.polkadot.io/#list/0xb3db41421702df9a7fcac62b53ffeac85f7853cc4e689e0b93aeb3db18c09d82)

🔰[Our Telegram Channel](https://t.me/CryptoSailorsAnn)

🔰[Our WebSite](cryptosailors.tech)
