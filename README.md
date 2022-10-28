<p style="font-size:14px" align="right">
<a href="https://t.me/PemulungAirdropID" target="_blank">Join our telegram <img src="https://user-images.githubusercontent.com/72949170/194228482-0f875615-e155-4b12-8716-8111addd6cba.jpg" width="30"/></a>
</p>

<p align="center">
  <img height="auto" height="auto" src="https://user-images.githubusercontent.com/72949170/198543164-b019a174-2404-456d-9edf-53e0d9a71204.png">
</p>

# MANDE TESTNET TUTORIAL

|  Komponen |  Persyaratan Minimum |
| ------------ | ------------ |
| CPU  | 2  |
| RAM | 2 GB  |
| Penyimpanan  | 100 GB |
| OS | Ubuntu 20.04 |

Official Link :
> [Official Document](https://github.com/mande-labs)

Explorer :
> [Explorer](https://explorer.stavr.tech/mande-chain)


## 1. Update Packages 
```
sudo apt update && sudo apt upgrade
```

## 2. Install Node
```
wget -O mnd.sh https://raw.githubusercontent.com/muhamad-ramadhani/mande/main/mnd.sh && chmod +x mnd.sh && ./mnd.sh
```

lalu ikuti command-command di bawah

muat variabel ke dalam sistem
```
source $HOME/.bash_profile
```

sinkron blok
```
mande-chaind status 2>&1 | jq .SyncInfo
```

![image](https://user-images.githubusercontent.com/72949170/198545540-677f28dc-1217-443a-bcd0-db4f18031bfc.png)

## 3. Create Wallet
```
mande-chaind keys add $WALLET
```

ganti ```$WALLET``` Jadi nama wallet kalian
![image](https://user-images.githubusercontent.com/72949170/198546079-2477fd12-c420-4669-8315-055c10bc30e4.png)


## 4. Save info Wallet

```
MANDE_WALLET_ADDRESS=$(mande-chaind keys show $WALLET -a)
MANDE_VALOPER_ADDRESS=$(mande-chaind keys show $WALLET --bech val -a)
echo 'export MANDE_WALLET_ADDRESS='${MANDE_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export MANDE_VALOPER_ADDRESS='${MANDE_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## 5. Request Faucet di discord mereka

https://discord.gg/rsswRVT3mU

command

```
$request <YOUR_WALLET_ADDRESS> theta
```

## 6. Create Validator
Setelah faucet landing, lanjut ke step ini

```
mande-chaind query bank balances $MANDE_WALLET_ADDRESS
```

Ganti ```$MANDE_WALLET_ADDRESS``` jadi address mande kalian
(Jika balance masih kosong tunggu sinkron dulu)

![image](https://user-images.githubusercontent.com/72949170/198547701-8ce4d8f1-776c-439b-8c15-a6f4d4b21071.png)

![image](https://user-images.githubusercontent.com/72949170/198547865-ac4f71dd-f6a9-4309-b982-3f995241f454.png)

Jalankan validator

```
mande-chaind tx staking create-validator \
--chain-id mande-testnet-1 \
--amount 0cred \
--identity xxxxxxxx \
--website "xxxxxxxx" \
--details="xxxxxxxx" \
--pubkey "$(mande-chaind tendermint show-validator)" \
--from YOUR_WALLET \
--moniker="YOUR_MONIKER" \
--fees 1000mand
```

Ganti ```YOUR_WALLET``` jadi address kalian
Ganti ```YOUR_MONIKER``` jadi nama wallet kalian




## USEFUL COMMAND (Bukan termasuk step tutor)

# Service management
Check logs

```
journalctl -fu mande-chaind -o cat
```

Start service

```
sudo systemctl start mande-chaind
```

Stop service

```
sudo systemctl stop mande-chaind
```

Restart service

```
sudo systemctl restart mande-chaind
```

# Node info
Synchronization info

```
mande-chaind status 2>&1 | jq .SyncInfo
```

Validator info

```
mande-chaind status 2>&1 | jq .ValidatorInfo
```

Node info

```
mande-chaind status 2>&1 | jq .NodeInfo
```

Show node id

```
mande-chaind tendermint show-node-id
```

# Wallet operations

List of wallets

```
mande-chaind keys list
```

Recover wallet

```
mande-chaind keys add <wallet> --recover
```

Delete wallet

```
mande-chaind keys delete <wallet>
```

Get wallet balance

```
mande-chaind query bank balances <address>
```

Transfer funds

```
mande-chaind tx bank send <FROM ADDRESS> <TO_MANDE_WALLET_ADDRESS> 10000000mand
```

Voting

```
mande-chaind tx gov vote 1 yes --from <wallet> --chain-id=mande-testnet-1
```

# Staking, Delegation and Rewards

Delegate stake

```
mande-chaind tx staking delegate <mande valoper> 10000000mand --from=<wallet> --chain-id=mande-testnet-1 --gas=auto
```

Redelegate stake from validator to another validator

```
mande-chaind tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 10000000mand --from=<wallet> --chain-id=mande-testnet-1 --gas=auto
```

Withdraw all rewards

```
mande-chaind tx distribution withdraw-all-rewards --from=<wallet> --chain-id=mande-testnet-1 --gas=auto
```

Withdraw rewards with commision

```
mande-chaind tx distribution withdraw-rewards <mande valoper> --from=<wallet> --commission --chain-id=mande-testnet-1
```

# Validator management

Edit validator

```
mande-chaind tx staking edit-validator \
  --moniker=<moniker> \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=mande-testnet-1 \
  --from=<wallet>
```

Unjail validator

```
mande-chaind tx slashing unjail \
  --broadcast-mode=block \
  --from=<wallet> \
  --chain-id=mande-testnet-1 \
  --gas=auto
```

Delete node

```
sudo systemctl stop mande-chaind && \
sudo systemctl disable mande-chaind && \
rm /etc/systemd/system/mande-chaind.service && \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf .mande-chain && \
rm -rf $(which mande-chaind)
```
