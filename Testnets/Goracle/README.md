<p align="center">
 <img src="https://i.postimg.cc/V6VkvYj9/photo-2023-03-28-13-34-31.jpg"/></a>
</p>

# Goracle testnet node installation guide.

## 1. Requirements.
#### Official 
- No Official info

#### Our Recommendations
- I recommend [Hetzner CX11](https://hetzner.cloud/?ref=NY9VHC3PPsL0)
- I recommend for convenience the SSH terminal - [MobaXTerm](https://mobaxterm.mobatek.net/download.html).

## 2. Server preparation.
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install make clang pkg-config libssl-dev libclang-dev build-essential git curl ntp jq llvm tmux htop screen unzip cmake -y
```
## 3. Install golang go
Use [this guide](https://github.com/CryptoSailors/cryptosailors-tools/tree/main/Install%20Golang%20%22Go%22#2-if-you-installing-golang-go-on-clear-server-you-need-input-following-commands) to install golang go using the second section.

## 4. Install docker and docker-compose
```
sudo apt install docker.io -y
git clone https://github.com/docker/compose
cd compose
latestTag=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep '.tag_name'|cut -d\" -f4)
echo $latestTag
git checkout $latestTag
make
cd ~
sudo mv compose/bin/build/docker-compose /usr/bin/docker-compose
chmod +x /usr/bin/docker-compose
docker-compose version
```
## 3. Install a node
```
wget https://staging.dev.goracle.io/downloads/latest-staging/goracle
sudo mv goracle /usr/bin
sudo chmod u+x /usr/bin/goracle
```
```
goracle init
```
You will be guided through the steps with text prompts and messages. You can abort the process at any time by pressing Ctrl-C. To start over, rename or delete the produced config file located at ~/.goracle

<p align="center">
 <img src="https://img1.teletype.in/files/00/50/00502481-5fac-4ba7-86eb-3feb511f6ddb.png"/></a>
</p>

Connect your pera wallet and prepare your phone. Make sure that you switch network to testnet on your phone and have tokens on your account and Participation key. You can use [this faucet if you need](https://bank.testnet.algorand.network/).

<p align="center">
 <img src="https://img4.teletype.in/files/f7/34/f734da10-8e2b-448a-a0db-c9b19aa2c370.png"/></a>
</p>

## 5. Start your node
```
sudo goracle docker-start --background
```
Check you logs
```
sudo docker logs goracle-nr -f --tail 100
```


#
👉[Hetzner](https://hetzner.cloud/?ref=NY9VHC3PPsL0).

👉[SSH terminal MobaxTerm](https://mobaxterm.mobatek.net/download.html)

👉[Discord](https://discord.gg/ukzSEdhCzF)

👉[WebSite](https://www.goracle.io/)

👉[Official guide](https://docs.goracle.io/technical-documentation/)

🔰[Our Telegram Channel](https://t.me/CryptoSailorsAnn)

🔰[Our WebSite](cryptosailors.tech)

🔰[Our Twitter](https://twitter.com/Crypto_Sailors)

#### Guide created by 

Pavel-LV | C.Sailors#7698 / @SeaInvestor
