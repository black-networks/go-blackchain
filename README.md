## Go BlackChain

Official Golang execution layer implementation of the BLACK protocol.


Automated builds are available for stable releases and the unstable master branch. Binary
archives are published at https://gblack.blackproject.space/downloads/.

## Building the source

For prerequisites and detailed build instructions please read the [Installation Instructions]().

Building `gblack` requires both a Go (version 1.18 or later) and a C compiler. You can install
them using your favourite package manager. Once the dependencies are installed, run

```shell
$ gblack --config /path/to/your_config.toml
```
To get an idea of how the file should look like you can use the dumpconfig subcommand to export your existing configuration:

```shell 
$ gblack --your-favourite-flags dumpconfig
```
Note: This works only with geth v1.6.0 and above.

### Programmatically interfacing gblack nodes

As a developer, sooner rather than later you'll want to start interacting with gblack and the Black network via your own programs and not manually through the console. To aid this, geth has built-in support for a JSON-RPC based APIs (standard APIs and gblack specific APIs). These can be exposed via HTTP, WebSockets and IPC (UNIX sockets on UNIX based platforms, and named pipes on Windows).

The IPC interface is enabled by default and exposes all the APIs supported by gblack, whereas the HTTP and WS interfaces need to manually be enabled and only expose a subset of APIs due to security reasons. These can be turned on/off and configured as you'd expect.

HTTP based JSON-RPC API options:
```shell
--http Enable the HTTP-RPC server

--http.addr HTTP-RPC server listening interface (default: localhost)

--http.port HTTP-RPC server listening port (default: 8545)

--http.api API's offered over the HTTP-RPC interface (default: eth,net,web3)

--http.corsdomain Comma separated list of domains from which to accept cross origin requests (browser enforced)

--ws Enable the WS-RPC server

--ws.addr WS-RPC server listening interface (default: localhost)

--ws.port WS-RPC server listening port (default: 8546)

--ws.api API's offered over the WS-RPC interface (default: eth,net,web3)

--ws.origins Origins from which to accept WebSocket requests

--ipcdisable Disable the IPC-RPC server

--ipcapi API's offered over the IPC-RPC interface (default: admin,debug,eth,miner,net,personal,txpool,web3)

--ipcpath Filename for IPC socket/pipe within the datadir (explicit paths escape it)
```
You'll need to use your own programming environments' capabilities (libraries, tools, etc) to connect via HTTP, WS or IPC to a geth node configured with the above flags and you'll need to speak JSON-RPC on all transports. You can reuse the same connection for multiple requests!


Note: Please understand the security implications of opening up an HTTP/WS based transport before doing so! Hackers on the internet are actively trying to subvert BlackChain nodes with exposed APIs! Further, all browser tabs can access locally running web servers, so malicious web pages could try to subvert locally available APIs!

### Running a private miner

Mining on the public Black network is a complex task as it's only feasible using GPUs, requiring an OpenCL or CUDA enabled ethminer instance. For information on such a setup, please consult the EtherMining subreddit and the ethminer repository.

In a private network setting, however, a single CPU miner instance is more than enough for practical purposes as it can produce a stable stream of blocks at the correct intervals without needing heavy resources (consider running on a single thread, no need for multiple ones either). To start a gblack instance for mining, run it with all your usual flags, extended by:
```shell
$ gblack <usual-flags> --mine --miner.threads=1 --miner.etherbase=0x0000000000000000000000000000000000000000
```
Which will start mining blocks and transactions on a single CPU thread, crediting all proceedings to the account specified by ```shell --miner.etherbase ```. You can further tune the mining by changing the default gas limit blocks converge to (```shell --miner.targetgaslimit ```)and the price transactions are accepted at (```shell --miner.gasprice ```).
