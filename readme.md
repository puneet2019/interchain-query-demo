# interchainquerydemo
**ICQ Demo** 

## Disclaimer

The following repository and [`x/interquery`](./x/interquery/) module serves as an example and is used to exercise the functionality of Interchain Accounts end-to-end for development purposes only.
This module **SHOULD NOT** be used in production systems

## Overview 

The following repository contains a basic example of an Interchain Queries module and serves as a developer guide for teams that wish to use interchain queries functionality.

### Developer Documentation

Interchain Queries developer docs can be found here

https://github.com/strangelove-ventures/ibc-go/blob/feature/icq_implementation/modules/apps/icq/README.md

## Demo

### Start the two instances of demo chain with following commands

```bash 
ignite chain serve -c sender.yml --reset-once
```

```bash 
ignite chain serve -c receiver.yml --reset-once
```

### Configure and start the relayer

```bash
rm -rf ~/.ignite/relayer
```


```bash
ignite relayer configure -a \
--source-rpc "http://localhost:26659" \
--source-faucet "http://localhost:4500" \
--source-port "interquery" \
--source-gasprice "0.0stake" \
--source-gaslimit 5000000 \
--source-prefix "cosmos" \
--source-version "icq-1" \
--target-rpc "http://localhost:26559" \
--target-faucet "http://localhost:4501" \
--target-port "icqhost" \
--target-gasprice "0.0stake" \
--target-gaslimit 300000 \
--target-prefix "cosmos"  \
--target-version "icq-1"
```

```bash
ignite relayer connect
```

### Send the query to "receiver" chain

```bash
interchain-query-demod tx interquery send-query-all-balances channel-0 cosmos1ez43ye5qn3q2zwh8uvswppvducwnkq6w6mthgl --chain-id=sender --node=tcp://localhost:26659 --home ~/.sender --from alice
```

### See the result of packet 1

```bash
interchain-query-demod query interquery query-state 1 --chain-id=sender --node=tcp://localhost:26659
```                                         