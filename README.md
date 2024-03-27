# Self-Chain

Website - https://selfchain.xyz/

Twitter - https://twitter.com/selfchainxyz

Telegram - https://t.me/selfchainxyz

Discord -https://discord.com/invite/selfchainxyz?ref=blog.selfchain.xyz

Docs: https://docs.selfchain.xyz/

Staking: https://staking.selfchain.xyz/

Self Chain Incentivized Testnet 2: https://blog.selfchain.xyz/self-chain-incentivized-testnet-2/

# 1. Minimum hardware requirement

4 Cores, 8G Ram, 200G SSD, Ubuntu 22.04

# 2. Auto Install
```
sudo apt install curl -y && source <(curl -s https://nodesync.top/selfchain_auto)
```
## 2.1 Wallet
Add New Wallet Key
```
selfchaind keys add wallet
```
Recover existing key
```
selfchaind keys add wallet --recover
```
List All Keys
```
selfchaind keys list
```
## 2.2 Query Wallet Balance
```
selfchaind q bank balances $(selfchaind keys show wallet -a)
```
## 2.3 Check sync status
False is synced
```
selfchaind status 2>&1 | jq .SyncInfo.catching_up
```
## 2.4 Create Validator
Change your info
```
selfchaind tx staking create-validator \
  --amount 1000000uself \
  --pubkey $(selfchaind tendermint show-validator) \
  --moniker "MONIKER" \
  --identity="YOUR_KEYBASE_ID" \
  --details="YOUR_DETAILS" \
  --website="YOUR_WEBSITE_URL" \
  --chain-id self-dev-1 \
  --commission-rate="0.05" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation "1" \
  --gas-prices="0.005uself" \
  --gas="auto" \
  --gas-adjustment="1.5" \
  --from wallet \
  -y
```
## 2.5 Edit Existing Validator 
```
selfchaind tx staking edit-validator \
--new-moniker="MONIKER" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id self-dev-1 \
--commission-rate=0.05 \
--gas-prices="0.005uself" \
--gas="auto" \
--gas-adjustment="1.5" \
--from wallet \
-y
```
## 2.6 Delegate Token to your own validator
```
selfchaind tx staking delegate $(selfchaind keys show wallet --bech val -a) 1000000uself --from wallet --chain-id self-dev-1 --gas-prices=0.005uself --gas-adjustment 1.5 --gas auto -y
```
## 2.7 Withdraw rewards and commission from your validator
```
selfchaind tx distribution withdraw-rewards $(selfchaind keys show wallet --bech val -a) --commission --from wallet --chain-id self-dev-1 --gas-prices=0.005uself --gas-adjustment 1.5 --gas auto -y
```
## 2.8 Unjail validator
```
selfchaind tx slashing unjail --from wallet --chain-id self-dev-1 --gas-prices=0.005uself --gas-adjustment 1.5 --gas auto -y
```
## 2.9 Services Management
```
# Reload Service
sudo systemctl daemon-reload

# Enable Service
sudo systemctl enable selfchaind

# Disable Service
sudo systemctl disable selfchaind

# Start Service
sudo systemctl start selfchaind

# Stop Service
sudo systemctl stop selfchaind

# Restart Service
sudo systemctl restart selfchaind

# Check Service Status
sudo systemctl status selfchaind

# Check Service Logs
sudo journalctl -u selfchaind -f --no-hostname -o cat
```
# 3. Backup Validator
Important
```
cat $HOME/.selfchain/config/priv_validator_key.json
```
# 4. Remove node
```
sudo systemctl stop selfchaind && sudo systemctl disable selfchaind && sudo rm /etc/systemd/system/selfchaind.service && sudo systemctl daemon-reload && rm -rf $HOME/.selfchain && sudo rm -rf $(which selfchaind)
```
# Share peers my community
```
cd $HOME
peers=$(curl -s https://raw.githubusercontent.com/nodesynctop/Self-Chain/main/peers.txt)
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.selfchain/config/config.toml
sudo systemctl restart selfchaind && sudo journalctl -u selfchaind -f --no-hostname -o cat
```

