# osmosis-demo

## Re: Zero - Starting Cosmos Blocks From Zero

The goal of this workshop is to run a single validator testnet on your local environment.

## 0. Pre-req
- Have go installed to the latest version

## 1. Clone Osmosis repo, build binary
Although in this workshop we would be practicing with Osmosis repo, keep in mind that the same commands apply to all other cosmos-sdk chains.

```sh
git clone https://github.com/osmosis-labs/osmosis.git
cd osmosis

make install
make build
```

## 2. Initialize genesis.json file that would se the initial state of chain
```sh
osmosisd init my-node --chain-id my-chain
```

## 3. Create key for your validator account
```sh
osmosisd keys add my-account --keyring-backend=test
```
Daemon uses keychains to manage keys: the keyring-backend test flag enables us to use the keyrings in the test mode

## 4. Add genesis account
One of the essential step in starting a chain is to have an account that the chain would start with. Why would we need to have a genesis account? - validators and blocks!

```sh 
osmosisd add-genesis-account $(./osmosisd keys show my-account -a --keyring-backend=test) 1000000000000uatom,100000000000uosmo,100000000000uion,100000000000valtoken,100000000000stake,10000foo,10000bar --keyring-backend=test
```

## 5. Create gentx (with validator starting delegation amount)
```sh
./osmosisd gentx my-account 10000000000stake --chain-id my-chain --keyring-backend=test
```
Gen tx is a set of transactions where it is executed in block height 0 to create appropriate state at the start of the chain.

The amount specified is the amount we will have staked from our account.

Running this command generates a json file in our home directory(~/.osmosisd)

## 6. Collect gentx
```sh
./osmosisd collect-gentxs
```
In practice, we would collect the gentx json file created from different “validators”. Then we run this command, in which then cosmos sdk would create initial state off of the genesis transactions.

## 7. LFG!
```sh
osmosisd start
```

## 8. Check your rewards instantly!

```sh
osmosisd query distribution validator-outstanding-rewards osmovaloper19f438whxuvxfkepsa7sk388p38qt9su2mx3y5h
```

## 9. Check my balance(before claiming my rewards):
```sh
osmosisd q bank balances osmo155248g4ck8ugjqjgapk4zn0arnjt6sepsn4zgq
```

## 10. Claim my rewards:
```sh
osmosisd tx distribution withdraw-rewards osmovaloper155248g4ck8ugjqjgapk4zn0arnjt6sep2yapl8 --from osmo155248g4ck8ugjqjgapk4zn0arnjt6sepsn4zgq --chain-id my-chain --fees 875stake
```

## 11. Check my balance (after claiming my rewards)
```sh
osmosisd query bank balances osmo155248g4ck8ugjqjgapk4zn0arnjt6sepsn4zgq
```
