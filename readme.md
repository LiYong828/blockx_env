# run node
```sh
git clone https://github.com/defi-ventures/bcx-testnet-5.git blockx
cd blockx
git checkout main
make install
cd $HOME
blockxd config chain-id blockx_12345-1
blockxd config keyring-backend file
rm -rf $HOME/.blockxd
blockxd init LYNODE --chain-id blockx_12345-1 --keyring-backend file
git clone https://github.com/liyong828/blockx_env.git
cd blockx_env
cp genesis.json $HOME/.blockxd/config
cd $HOME
vim $HOME/.blockxd/config/config.toml
(insert seednode: 50d5f80efc5a24a919a5d0f05e96728e19ee22b2@142.93.202.64:26656,7a35db3f64df47032e95289d3e3d4b55608acb61@143.93.3.163:26656)
blockxd start --minimum-gas-prices=1000000000abcx
```

# create validator
```sh
blockxd tx staking create-validator   --amount=1000000000000000000000000abcx   --pubkey=$(blockxd tendermint show-validator)   --moniker="LYNODE"   --chain-id=blockx_12345-1   --commission-rate="0.05"   --commission-max-rate="0.10"   --commission-max-change-rate="0.01"   --min-self-delegation="1000000"   --gas="300000"   --gas-prices="1000000000abcx"   --from=ly --keyring-backend file
```
# delegate
```sh
blockxd tx staking delegate blockxvaloper1xfhs3qkrw8ysdawwc4ytv9z7ls50gftua287d5 10000000000000000000abcx --from blockx14mwtus7ha6e6kdhytrl4km8au9lfvugdtlvjxh --chain-id blockx_12345-1 --fees 210000000000000abcx
```
# check delegation reward
```sh
blockxd query distribution rewards blockx14gct2nk2d9f6rs0edsyg4nuavkng8t8u0qf6dw blockxvaloper14gct2nk2d9f6rs0edsyg4nuavkng8t8u8zkv0x --keyring-backend file --chain-id blockx_12345-1

blockxd query distribution rewards blockx14gct2nk2d9f6rs0edsyg4nuavkng8t8u0qf6dw blockxvaloper14fc6scl249kmctjjfp4ym9ad4n8yyddy7zrvcr --keyring-backend file --chain-id blockx_12345-1
```

# transfer token and check balance
```sh
blockxd tx bank send blockx14gct2nk2d9f6rs0edsyg4nuavkng8t8u0qf6dw blockx14fc6scl249kmctjjfp4ym9ad4n8yyddykqu66t 9000000000000000000000000abcx --keyring-backend file --chain-id blockx_12345-1 --gas 210000 --gas-prices 1000000000abcx

blockxd query bank balances blockx14gct2nk2d9f6rs0edsyg4nuavkng8t8u0qf6dw --keyring-backend file --chain-id blockx_12345-1
```

# export private key
```sh
blockxd keys unsafe-export-eth-key seedkey2 --keyring-backend file
```

# withdraw rewards
```sh
blockxd tx distribution withdraw-all-rewards --from seedkey2 --gas auto --gas-adjustment 1.5 --gas-prices 1000000000abcx
```

# reward(gas fee) distribution check
```sh
# check balance
blockxd query bank balances blockx14fc6scl249kmctjjfp4ym9ad4n8yyddykqu66t  --keyring-backend file --chain-id blockx_12345-1
#8000000000610080000000000

# check reward
blockxd query distribution rewards blockx14fc6scl249kmctjjfp4ym9ad4n8yyddykqu66t  blockxvaloper14fc6scl249kmctjjfp4ym9ad4n8yyddy7zrvcr --keyring-backend file --chain-id blockx_12345-1
#40965750000000

# withdraw reward
blockxd tx distribution withdraw-all-rewards --from seedkey2 --gas auto --gas-adjustment 1.5 --gas-prices 1000000000abcx

# check balance again to check 
blockxd query bank balances blockx14fc6scl249kmctjjfp4ym9ad4n8yyddykqu66t  --keyring-backend file --chain-id blockx_12345-1
#8000000000423318750000000

# tx gas fee was 227727000000000

# old balance + reward - gas fee = updated balance
# 8000000000610080000000000 + 40965750000000 - 227727000000000 = 8000000000423318750000000

```
# seednode1
* seedkey1, seednode1
* delegator_address: blockx14gct2nk2d9f6rs0edsyg4nuavkng8t8u0qf6dw,
* validator_address:
blockxvaloper14gct2nk2d9f6rs0edsyg4nuavkng8t8u8zkv0x
* mnemonic: reject melt toward decade boy elbow aspect symbol snow property plastic carbon tree spot pill helmet trust destroy warrior accuse report lumber motion medal
* nodeid: 50d5f80efc5a24a919a5d0f05e96728e19ee22b2

# seednode2
* seedkey2, seednode2
* delegator_address: blockx14fc6scl249kmctjjfp4ym9ad4n8yyddykqu66t,
* validator_address:
blockxvaloper14fc6scl249kmctjjfp4ym9ad4n8yyddy7zrvcr
* mnemonic: margin welcome toast crush expand elite beyond beef vital canyon kingdom snap ill tank remember diagram mad tunnel monkey crystal rebel riot wonder mountain
* nodeid: 7a35db3f64df47032e95289d3e3d4b55608acb61

gas fee
31500000147000
210000000000000
