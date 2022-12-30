## Go BlackChain

Official Golang execution layer implementation of the BLACK protocol.


Automated builds are available for stable releases and the unstable master branch. Binary
archives are published at https://gblack.blackproject.space/downloads/.

## Building the source

For prerequisites and detailed build instructions please read the [Installation Instructions]().

Building `gblack` requires both a Go (version 1.18 or later) and a C compiler. You can install
them using your favourite package manager. Once the dependencies are installed, run

```shell
make gblack
```

or, to build the full suite of utilities:

```shell
make all
```

## Executables

The go-ethereum project comes with several wrappers/executables found in the `cmd`
directory.

|    Command    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| :-----------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  **`gblack`**   | Our main BlackChain CLI client. It is the entry point into the Black network (main-, test- or private net), capable of running as a full node (default), archive node (retaining all historical state) or a light node (retrieving data live). It can be used by other processes as a gateway into the Black network via JSON RPC endpoints exposed on top of HTTP, WebSocket and/or IPC transports. `gblack --help` and the [CLI page]() for command line options.          |
|   `clef`    | Stand-alone signing tool, which can be used as a backend signer for `geth`.  |
|   `devp2p`    | Utilities to interact with nodes on the networking layer, without running a full blockchain. |
|   `abigen`    | Source code generator to convert Ethereum contract definitions into easy-to-use, compile-time type-safe Go packages. It operates on plain [Black contract ABIs](https://docs.soliditylang.org/en/develop/abi-spec.html) with expanded functionality if the contract bytecode is also available. However, it also accepts Solidity source files, making development much more streamlined.. |
|  `bootnode`   | Stripped down version of our Black client implementation that only takes part in the network node discovery protocol, but does not run any of the higher level application protocols. It can be used as a lightweight bootstrap node to aid in finding peers in private networks.                                                                                                                                                                                                                                                                 |
|     `evm`     | Developer utility version of the EVM (Ethereum Virtual Machine) that is capable of running bytecode snippets within a configurable environment and execution mode. Its purpose is to allow isolated, fine-grained debugging of EVM opcodes (e.g. `evm --code 60ff60ff --debug run`).                                                                                                                                                                                                                                                                     |
|   `rlpdump`   | Developer utility tool to convert binary RLP) dumps (data encoding used by the Black protocol both network as well as consensus wise) to user-friendlier hierarchical representation (e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`).                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

## Running `gblack`

Going through all the possible command line flags is out of scope here (please consult our
[CLI Wiki page]()),
but we've enumerated a few common parameter combos to get you up to speed quickly
on how you can run your own `geth` instance.

### Hardware Requirements

Minimum:

* CPU with 2+ cores
* 4GB RAM
* 50GB free storage space to sync the Mainnet
* 8 MBit/sec download Internet service

Recommended:

* Fast CPU with 4+ cores
* 16GB+ RAM
* High-performance SSD with at least 128GB of free space
* 25+ MBit/sec download Internet service

### Full node on the main Black network

By far the most common scenario is people wanting to simply interact with the Ethereum
network: create accounts; transfer funds; deploy and interact with contracts. For this
particular use case, the user doesn't care about years-old historical data, so we can
sync quickly to the current state of the network. To do so:

```shell
$ gblack console
```

This command will:
 * Start `gblack` in snap sync mode (default, can be changed with the `--syncmode` flag),
   causing it to download more data in exchange for avoiding processing the entire history
   of the Gblack network, which is very CPU intensive.
 * Start the built-in interactive [JavaScript console](),
   (via the trailing `console` subcommand) through which you can interact using [`web3` methods]() 
   (note: the `web3` version bundled within `geth` is very old, and not up to date with official docs),
   as well as `geth`'s own [management APIs]().
   This tool is optional and if you leave it out you can always attach it to an already running
   `gblack` instance with `gblack attach`.

