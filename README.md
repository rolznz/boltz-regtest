# Boltz Regtest

An environment for local development and testing, successor of [legend-regtest-enviroment](https://github.com/BoltzExchange/legend-regtest-enviroment).

## Prerequisites
[Docker](https://docs.docker.com/engine/install/) or [Orbstack](https://orbstack.dev/) for Apple Silicon based Macs.
When using OrbStack on macOS, set `export DOCKER_DEFAULT_PLATFORM=linux/amd64` before starting.

## Usage

```bash
./start.sh
```

```bash
./stop.sh
```

- Web App: [http://localhost:8080](http://localhost:8080)
- API: [http://localhost:9001](http://localhost:9001)

Data dirs for the services are stored in `./data` folder.

### Scripts container

```bash
docker exec -it boltz-scripts bash
```

- bitcoin-cli-sim-client
- bitcoin-cli-sim-server
- elements-cli-sim-client
- elements-cli-sim-server
- boltzcli-sim ([boltz-client](https://github.com/BoltzExchange/boltz-client))
- lightning-cli-sim
- lncli-sim

Since there are two lnd and two cln instances, use `lncli-sim 1` or `lightning-cli-sim 1` to interact with the first instance and `lncli-sim 2` or `lightning-cli-sim 2` to interact with the second.

Or alternatively, you can `source aliases.sh` to have these convenience scripts available on the host machine.

### Block explorers

[Esplora](https://github.com/Blockstream/esplora) is running for the Bitcoin Core and Elements regtest:

- Bitcoin: [http://localhost:4002](http://localhost:4002)
- Elements: [http://localhost:4003](http://localhost:4003)

[Otterscan](https://github.com/otterscan/otterscan) is used as block explorer for Anvil: [http://localhost:5100](http://localhost:5100)

### Your first swap

Run `./start.sh`

then run bash on the container

```bash
docker exec -it boltz-scripts bash
```

check your bitcoin wallet balance

```bash
bitcoin-cli-sim-client getbalance
```

#### Connecting to the network

connect to bitcoind via rpc (`127.0.0.1:18433`, using cookie auth or rpc user:password can be extracted with `cat /root/.bitcoin/regtest/.cookie`) or esplora (`http://localhost:4002/api`)

#### Top up your wallet

Generate an address in your wallet and send 1 BTC e.g.

```bash
bitcoin-cli-sim-client sendtoaddress bcrt1qfzjp72mft2kcx49w49l5v6vdegr7lff86j08w7 1
```

Then mine a block for the tx to confirm

```bash
bitcoin-cli-sim-client -generate 1
```

#### Open a channel with LND-1

1. Get the identity_pubkey:

```bash
lncli-sim 1 getinfo
```

2. Connect to the peer and open a channel:

`023a6159c3be21ef992d89d9579c589c9f27308e61ae1dcb609170c13af09c0504@127.0.0.1:29735`

3. Mine some blocks for the tx to confirm

```bash
bitcoin-cli-sim-client -generate 6
```

Now you should be able to swap out!
