## CHEAT SHEET VALIDATOR, GANTI KATA "seid" DENGAN PROJECT YANG SEDANG SEKARANG KAMU KERJAKAN, JANGAN LUPA TAMBAHKAN HURUF "D" DI BELAKANGNYA.
## CONTOH HURUF "D" DI BELAKANG SEPERTI: seid (sei), quicksilverd (quicksilver), stride (strided), dll.





### Get list of validators
```
seid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

## Get currently connected peer list with ids
```
curl -sS http://localhost:${SEI_PORT}657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

## Usefull commands
### Service management
Check logs
```
journalctl -fu seid -o cat
```

Start service
```
sudo systemctl start seid
```

Stop service
```
sudo systemctl stop seid
```

Restart service
```
sudo systemctl restart seid
```
Check if you installed using cosmovisor or service | Example /root/.sei/target/release/ (this installed using service)
```
which seid
```


### Node info
Synchronization info
```
seid status 2>&1 | jq .SyncInfo
```

Validator info
```
seid status 2>&1 | jq .ValidatorInfo
```

Node info
```
seid status 2>&1 | jq .NodeInfo
```

Show node id
```
seid tendermint show-node-id
```

### Wallet operations
List of wallets
```
seid keys list
```

Recover wallet
```
seid keys add $WALLET --recover
```

Delete wallet
```
seid keys delete $WALLET
```

Get wallet balance
```
seid query bank balances $SEI_WALLET_ADDRESS
```

Transfer funds
```
seid tx bank send $SEI_WALLET_ADDRESS <TO_SEI_WALLET_ADDRESS> 10000000usei
```

### Voting
```
seid tx gov vote 1 yes --from $WALLET --chain-id=$SEI_CHAIN_ID
```

### Staking, Delegation and Rewards
Delegate stake
```
seid tx staking delegate $SEI_VALOPER_ADDRESS 10000000usei --from=$WALLET --chain-id=$SEI_CHAIN_ID --gas=auto
```

Redelegate stake from validator to another validator
```
seid tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 10000000usei --from=$WALLET --chain-id=$SEI_CHAIN_ID --gas=auto
```

Withdraw all rewards
```
seid tx distribution withdraw-all-rewards --from=$WALLET --chain-id=$SEI_CHAIN_ID --gas=auto
```

Withdraw rewards with commision
```
seid tx distribution withdraw-rewards $SEI_VALOPER_ADDRESS --from=$WALLET --commission --chain-id=$SEI_CHAIN_ID
```

### Validator management
Edit validator
```
seid tx staking edit-validator \
  --moniker=$NODENAME \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=$SEI_CHAIN_ID \
  --from=$WALLET
```

Unjail validator
```
seid tx slashing unjail \
  --broadcast-mode=block \
  --from=$WALLET \
  --chain-id=$SEI_CHAIN_ID \
  --gas=auto
```

### Delete node
This commands will completely remove node from server. Use at your own risk!
```
sudo systemctl stop seid
sudo systemctl disable seid
sudo rm /etc/systemd/system/sei* -rf
sudo rm $(which seid) -rf
sudo rm $HOME/.sei -rf
sudo rm $HOME/sei-chain -rf
sed -i '/SEI_/d' ~/.bash_profile
```
