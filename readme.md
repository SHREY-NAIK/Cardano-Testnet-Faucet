# Build a Faucet :

### Compile ppbl-faucet-<YOUR TOKEN>.plutus

### Build Address
```
cardano-cli address build \
--payment-script-file ppbl-faucet-preprod-<YOUR TOKEN>.plutus \
--testnet-magic 1 \
--out-file ppbl-faucet-<YOUR TOKEN>.addr
```

### Prepare Datum :
```
cardano-cli transaction hash-script-data --script-data-value <SOME NUMBER>
> <OUTPUT>
```

### Build and Submit Locking Transaction :

Set Variables

```
SENDER
SENDERKEY
TXIN1=
TXIN2=
CONTRACTADDR=
DATUMHASH=
ASSET=
NUM_TOKENS=
=======
SENDER=
SENDERKEY=
TXIN1=
TXIN2=
CONTRACTADDR=
DATUMHASH=
ASSET=
NUM_TOKENS= how many tokens do you want to lock?
```

Build a Locking Transaction:
```
cardano-cli transaction build \
--babbage-era \
--tx-in $TXIN1 \
--tx-in $TXIN2 \
--tx-out $CONTRACTADDR+"1500000 + $NUM_TOKENS $ASSET" \
--tx-out $CONTRACTADDR+"2000000 + 500 $ASSET" \
--tx-out-datum-hash $DATUMHASH \
--change-address $SENDER \
--protocol-params-file protocol.json \
--out-file tx-lock.raw \
--testnet-magic 1

cardano-cli transaction sign \
--signing-key-file $SENDERKEY \
--testnet-magic 1 \
--tx-body-file tx-lock.raw \
--out-file tx-lock.signed

cardano-cli transaction submit \
--tx-file tx-lock.signed \
--testnet-magic 1

```

##  Unlocking 
### Step By Step:
Create a `redeemer.json` file with the following contents:
{"constructor":0,"fields":[{"bytes":"<YOUR PUBKEYHASH HERE"}]}

## Build an Unlocking Transaction:

#### Set Variables
```
CONTRACT_TXIN=
AUTH_TOKEN_TXIN=
FEE_TXIN=
COLLATERAL=
PLUTUS_SCRIPT_FILE=
ASSET=
AUTH_TOKEN=
TOKENS_BACK_TO_CONTRACT=
CONTRACTADDR=
DATUMHASH=
```


#### Build Unlocking Transaction
```
cardano-cli transaction build \
--babbage-era \
--testnet-magic 1 \
--tx-in $AUTH_TOKEN_TXIN \
--tx-in $FEE_TXIN \ 
--tx-in $CONTRACT_TXIN \
--tx-in-script-file $PLUTUS_SCRIPT_FILE \
--tx-in-datum-value 1618 \
--tx-in-redeemer-file redeemer.json \
--tx-in-collateral $COLLATERAL \
--tx-out $MINTER+"1500000 + 1000 $ASSET" \
--tx-out $MINTER+"1500000 + 1 $AUTH_TOKEN" \
--tx-out $CONTRACTADDR+"2000000 + $TOKENS_BACK_TO_CONTRACT $ASSET" \
--tx-out-datum-hash $DATUMHASH \
--change-address $MINTER \
--protocol-params-file protocol.json \
--out-file unlock.raw

cardano-cli transaction sign \
--signing-key-file $SENDERKEY \
--testnet-magic 1 \
--tx-body-file unlock.raw \
--out-file unlock.signed

cardano-cli transaction submit \
--tx-file unlock.signed \
--testnet-magic 1
```
YOU CAN NOW UNBLOCK MY TOKENS NAMED rey FROM THE ppbl-front-end-Template!

![Screenshot from 2023-01-28 01-34-36](https://user-images.githubusercontent.com/47191058/215187297-4facc935-dcaf-4b9b-b7db-a07aa5d4d713.png)
