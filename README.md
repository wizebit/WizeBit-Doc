# WizeBit Blockchain 


Currently, WizeBit software development is carried out in the following areas:

### Blockchain | Storage | Desktop App | Raft | Web

Implementation of the phased development of a private (closed) BlockChain network that allows to host nodes on the main types of servers (dedicated servers, cloud storage, VPS, etc.). A node as the main entity of a Blockchain network must be equipped with a basic software and mathematical tools technique to participate in all the necessary BlockChain processes, namely:

* automatic self-determination of the node in the network;
* adding transactions;
* implementation of consensus;
* support of the mining process.

WizeBit Blockchain network have a software interface for desktop programs that allows to save files within the network while observing a number of security measures. The main purpose of the WizeBit Blockchain network to be developed is to ensure the maximum security, confidentiality and invulnerability of the information transferred to the storage. 

At the same time, to ensure the maximum productivity of saving and retrieving target information. Based on this the policy of choosing the algorithms used, existing prototypes, and implementation tools used is determined.


* algorithm for adding blocks - Proof-of-Stake;
* block adding mode - parallel on all nodes existing in the current time interval;
* based on the block adding mode the chain is built by merging chains of different servers 
* Network Scalability Principle - Sharding




# Blockchain (Install. Setup. Start)

`go version go1.9.2+`
`linux/mac os x required `

### Start cluster:

`install Docker-CE: https://www.docker.com/community-edition`

`install Docker Compose: https://docs.docker.com/compose/install/`

`run bash start_cluster.sh`

`Connect URL to blockchain node is localhost:4000`. 

`Connect URL to raft node is localhost:11001.`

Postman collection can be found in root folder the of the project.


# How It Works
### Blockchain Basic: blocks, transactions

Blockchain is just a database with certain structure: it’s an ordered, back-linked list. Which means that blocks are stored in the insertion order and that each block is linked to the previous one. This structure allows to quickly get the latest block in a chain and to (efficiently) get a block by its hash.

In blockchain it’s blocks that store valuable information, in particular, transactions, the essence of any cryptocurrency. Besides this, a block contains some technical information, like its version, current timestamp and the hash of the previous block.

A transaction is a combination of inputs and outputs. Inputs of a new transaction reference outputs of a previous transaction. Outputs are where coins are actually stored.

# Wize Wallet

In WizeBlock, your identity is a pair of private and public keys stored on your computer (or stored in some other place you have access to). A wallet is nothing but a key pair. In the construction of wallet a new key pair is generated with ECDSA which is based on elliptic curves. A private key is generated using the curve, and a public key is generated from the private key. One thing to notice: in elliptic curve based algorithms, public keys are points on a curve. This is a public key is a combination of X, Y coordinates.

If you want to send coins to someone, you need to know their address. But addresses (despite being unique) are not something that identifies you as the owner of a “wallet”. In fact, such addresses are a human-readable representation of public keys. The address generation algorithm utilizes a combination of open algorithms that take a public key and returns real Base58-based address.

Currently, WizeBlock has generating wallets on the WizeBlock node side, but in the next version wallets will generate on the user side in the Desktop application.




# Network
### Network nodes and their roles

Current WizeBlock network implementation has some simplification. Bitcoin network is decentralized, which means there’re no servers that do stuff and clients that use servers to get or process data. In our implementation, there will be centralization though.

### We’ll have three node roles:

### The central node (Master). 
This is the node all other nodes will connect to, and this is the node that’ll send data between other nodes.

### A miner node.
This node will store new transactions in mempool and when there’re enough of transactions, it’ll mine a new block.

### A wallet node. 
This node will be used to send coins between wallets. It’ll store a full copy of blockchain.



# Encryption Messages
### Nodes communicate by the means of messages.

When a new node is run, it gets several nodes from a DNS seed, and sends them version message. If node is not the central one, it must send version message to the central node to find out if its blockchain is outdated.

Next message getblocks means “show me what blocks you have” (in Bitcoin, it’s more complex). Pay attention, it doesn’t say “give me all your blocks”, instead it requests a list of block hashes.

WizeBlock uses inv to show other nodes what blocks or transactions current node has. Again, it doesn’t contain whole blocks and transactions, just their hashes.

Message getdata is a request for certain block or transaction, and it can contain only one block or transaction ID. The handler is straightforward: if they request a block, return the block; if they request a transaction, return the transaction. Notice, that we don’t check if we actually have this block or transaction.
Messages block and tx actually transfer the data.

# REST Service
### WizeBlock provides a REST service with next API:

Create Wallet `nodeAddress:nodePort/wallet/new` 
returns wallet info 
`private and public keys, base58-based address`

### Get Wallet
`(nodeAddress:nodePort/wallet/{wallet_address})`
 returns wallet details 
`(wallet balance)`

### Send Transaction 

`(nodeAddress:nodePort/send)` with POST parameters: from_address, to_address, amount value and `minenow flag`; 

minenow flag is used for mining new blocks, if it is true new block will mine, and if it false the Miner nodes receive the transaction and keep it in its memory pool and when there are enough transactions in the memory pool, the miner starts mining a new block.

# Mining

### Mining algorithm, proof-of-work

A key idea of blockchain is that one has to perform some hard work to put data in it. It is this hard work that makes blockchain secure and consistent. Also, a reward is paid for this hard work (this is how people get coins for mining).

In blockchain, some participants (miners) of the network work to sustain the network, to add new blocks to it, and get a reward for their work. As a result of their work, a block is incorporated into the blockchain in a secure way, which maintains the stability of the whole blockchain database. It’s worth noting that, the one who finished the work has to prove this.

This whole “do hard work and prove” mechanism is called proof-of-work. It’s hard because it requires a lot of computational power: even high-performance computers cannot do it quickly.

Proof-of-Work algorithms must meet a requirement: doing the work is hard, but verifying the proof is easy. A proof is usually handed to someone else, so for them, it shouldn’t take much time to verify it.
WizeBlock uses Hashcash, a Proof-of-Work algorithm that was initially developed to secure network. This is a brute force algorithm: you change the counter, calculate a new hash, check it, increment the counter, calculate a hash, etc.


### Implementation of the Proof-of-Stake algorithm for adding blocks.

The purpose is to implement the Proof-of-Stake algorithm for adding blocks to Wize BlockChain. Blocks should be added in parallel mode. Accordingly, proceeding from this algorithm will be implemented for the formation of consensus.

### Implementing the CLI 

WizeBit Blockchain network developed to supported sets of commands based on the language of GoLang (https://golang.org). This set of commands will cover all the necessary needs for servicing the node, server part on which the node is deployed, monitoring and analyzing the communication protocol between the nodes, monitoring and analyzing the data contained within the BlockChain network.

### The process of adding a transaction. 

Adding transactions will be performed on each node on mutually equal terms. The process of mining will consist in merging new blocks from all nodes into a single block and this block will be distributed to all existing nodes.




# Encryption
### Hashing algorithms

Hashing is a process of obtaining a hash for specified data. A hash is a unique representation of the data it was calculated on. A hash function is a function that takes data of arbitrary size and produces a fixed size hash. Hashing functions are widely used to check the consistency of data. Some software providers publish checksums in addition to a software package.

In blockchain, hashing is used to guarantee the consistency of a block. The input data for a hashing algorithm contains the hash of the previous block, thus making it impossible (or, at least, quite difficult) to modify a block in the chain: one has to recalculate its hash and hashes of all the blocks after it.
More about hashing: https://en.bitcoin.it/wiki/Block_hashing_algorithm

### ECDSA
WizeBlock uses elliptic curves to generate private keys. Elliptic curves is a complex mathematical concept, which we’re not going to explain in details here. What we need to know is that these curves can be used to generate really big and random numbers. The curve used by WizeBlock can randomly pick a number between 0 and 2²⁵⁶ (which is approximately 10⁷⁷, when there are between 10⁷⁸ and 10⁸² atoms in the visible universe). Such a huge upper limit means that it’s almost impossible to generate the same private key twice.
Also, WizeBlock uses ECDSA (Elliptic Curve Digital Signature Algorithm) algorithm to sign transactions.






# Data Storage

### Setup
`go version go1.9.2`

CLI application

`wizefs_cli` application is located here: `$GOPATH/src/bitbucket.org/udt/wizefs/cmd/wizefs_cli`
You should go to this directory and run `go build`.

Also, you can run from root directory `$GOPATH/src/bitbucket.org/udt/wizefs` this command:
`go build -o ./cmd/wizefs_cli/wizefs_cli -v ./cmd/wizefs_cli`or you can build it right in the root directory:
`go build -v ./cmd/wizefs_cli`

### gRPC Server and Client

gRPC applications are located at the grpc directory: `$GOPATH/src/bitbucket.org/udt/wizefs/grpc`

You should build `wizefs_mount` application before building gRPC Server and gRPC Client.

`wizefs_mount` application is located here: `$GOPATH/src/bitbucket.org/udt/wizefs/cmd/wizefs_mount`
You should go to this directory and run `go build`.

Also you can run from root directory `$GOPATH/src/bitbucket.org/udt/wizefs` this command:
`go build -o ./cmd/wizefs_mount/wizefs_mount -v ./cmd/wizefs_mount`

Then you should build 2 commands independently by going to the appropriate folder in advance: `grpc/server` and `grpc/client`.





# REST Service
REST Service is located at the rest directory: `$GOPATH/src/bitbucket.org/udt/wizefs/rest`
You should go to this directory and run `go build`.

Also you should build `wizefs_mount` application before REST Service. See more details in topic gRPC Server and Client.

### Setup


go version go1.9.2


### CLI application


`wizefs_cli` application is located here:
`$GOPATH/src/bitbucket.org/udt/wizefs/cmd/wizefs_cli`

You should go to this directory and run `go build`.

Also you can run from root directory `$GOPATH/src/bitbucket.org/udt/wizefs` this command:

`go build -o ./cmd/wizefs_cli/wizefs_cli -v ./cmd/wizefs_cli`

or you can build it right in the root directory:

`go build -v ./cmd/wizefs_cli`


### gRPC Server and Client


gRPC applications are located at the **grpc** directory:
`$GOPATH/src/bitbucket.org/udt/wizefs/grpc`

You should build **wizefs_mount application** before building gRPC Server and gRPC Client.

wizefs_mount application is located here:
`$GOPATH/src/bitbucket.org/udt/wizefs/cmd/wizefs_mount`

You should go to this directory and run `go build`.

Also you can run from root directory `$GOPATH/src/bitbucket.org/udt/wizefs` this command:

`go build -o ./cmd/wizefs_mount/wizefs_mount -v ./cmd/wizefs_mount`

Then you should build 2 commands independently by going to the appropriate folder in advance: `grpc/server` and `grpc/client`.

### REST Service

REST Service is located at the **rest** directory:
`$GOPATH/src/bitbucket.org/udt/wizefs/rest`

You should go to this directory and run `go build`.

Also you should build **wizefs_mount application** before REST Service. See more details in topic **gRPC Server and Client**.

### WizeFS Docker node (with REST Service running inside)

You can start WizeFS Docker node with `./start.sh`

### GUI application

See [GUI README](cmd/wizefs_ui/README.md)


### Flags


`--debug`

Enable debug output. Optional.

`--fg, -f`

Stay in the foreground. Optional.

`--notifypid`

Send USR1 to the specified process after successful mount. 
It used internally for daemonization.


## Common Info


`ORIGIN`

ORIGIN is single name of bucket. It is not directory with full path, just name or label for one of the many WizeFS buckets.

`Bucket`

Bucket for WizeFS is a synonym of FUSE-based filesystem. Currently there are 3 basic types of filesystems (and buckets):

1.  Loopback Filesystem (or simply LoopbackFS)
2.  Zipped Filesystem (or simply ZipFS) (read-only, in-memory)
3.  Loopback Zipped Filesystem (or simply LZFS).


## API (Command-line interface)


`create ORIGIN`

Create a new bucket. 
Now this command only checks if ORIGIN directory exists and create it if it is not exist. Also this command create config file for this bucket (wizefs.conf) and add this bucket to `created` map of common config (wizedb.conf).

### create Issues

* Check if bucket is (isn't) mounted. Perhaps should add flag for auto-mounting after creating

`delete ORIGIN`

Delete an existing bucket.
Now this command only checks if ORIGIN directory exists and delete it in this case with config file. Also this command delete bucket from `created` map of common config.

### delete Issues

* Check if bucket is mounted. Perhaps should add flag for auto-unmounting before deleting

`mount ORIGIN`

Mount an existing ORIGIN (directory or zip file) into MOUNTPOINT (this directory now is creating by application in the WizeFS root directory).
Also this command add bucket (with all needed data) to `mounted` map of common config (wizedb.conf).

`unmount ORIGIN`

Unmount an existing ORIGIN (application can search MOUNTPOINT by ORIGIN).
Also this command delete bucket from `mounted` map of common config.

`put FILE ORIGIN`

Upload FILE (you can use full path to the file here) to existing and mounted bucket with name (label) ORIGIN. Now it work only with directory-based bucket, but also you can experiment with LZFS bucket (zipped directory, with ORIGIN like archive.zip, currently only zip archive supported).

`get FILE ORIGIN`

Download FILE (you should use only filename now) from existing and mounted bucket with name (label) ORIGIN to the current directory. Now it work only with directory-based bucket, but also you can experiment with LZFS bucket (zipped directory, with ORIGIN like archive.zip, currently only zip archive supported).

`remove FILE ORIGIN`

Remove FILE (you should use only filename) from existing and mounted bucket with name (label) ORIGIN. Now it work only with directory-based bucket, but also you can experiment with LZFS bucket (zipped directory, with ORIGIN like archive.zip, currently only zip archive supported).


### API Commands Issues

* Add some other Filesystems API, like `check`, `list`
* Add Files API:  `search`
* Add Internal API: `verify`, `integrity`


## gRPC API methods


gRPC methods are identical to CLI commands, just first symbol is capitalized.

You can see gRPC Client code with all 6 commands working step by step.

```go
func main() {
	flag.Parse()

	var opts []grpc.DialOption
	opts = append(opts, grpc.WithInsecure())

	conn, err := grpc.Dial(*serverAddr, opts...)
	if err != nil {
		tlog.Fatal.Printf("fail to dial: %v", err)
	}
	defer conn.Close()
	client := pb.NewWizeFsServiceClient(conn)

	origin := "GRPC1"

	// Create
	tlog.Info.Printf("Request: Create. Origin: %s", origin)
	resp, err := client.Create(context.Background(), &pb.FilesystemRequest{Origin: origin})
	tlog.Info.Printf("Response: %v. Error: %v", resp, err)

	time.Sleep(3 * time.Second)

	// Mount
	tlog.Info.Printf("Request: Mount. Origin: %s", origin)
	resp, err = client.Mount(context.Background(), &pb.FilesystemRequest{Origin: origin})
	tlog.Info.Printf("Response: %v. Error: %v", resp, err)

	time.Sleep(3 * time.Second)

	// Put
	// TODO: HACK - just for local testing
	filepath := globals.OriginDirPath + "TESTDIR1/test.txt"
	content, err := readFile(filepath)
	if err == nil {
		tlog.Info.Printf("Request content: \n%s\n", content)

		tlog.Info.Printf("Request: Put. Origin: %s", origin)
		respPut, err := client.Put(context.Background(),
			&pb.PutRequest{
				Filename: "test.txt",
				Content:  content,
				Origin:   origin,
			})
		tlog.Info.Printf("Response: %v. Error: %v", respPut, err)
	}

	time.Sleep(3 * time.Second)

	// Get
	if err == nil {
		tlog.Info.Printf("Request: Get. Origin: %s", origin)
		respGet, err := client.Get(context.Background(),
			&pb.GetRequest{
				Filename: "test.txt",
				Origin:   origin,
			})
		tlog.Info.Printf("Error: %v", err)
		tlog.Info.Printf("Response message: %s.", respGet.Message)
		tlog.Info.Printf("Response contentb: \n%s\n", respGet.Content)
	}

	time.Sleep(3 * time.Second)

	// Unmount
	tlog.Info.Printf("Request: Unmount. Origin: %s", origin)
	resp, err = client.Unmount(context.Background(), &pb.FilesystemRequest{Origin: origin})
	tlog.Info.Printf("Response: %v. Error: %v", resp, err)

	time.Sleep(3 * time.Second)

	// Delete
	tlog.Info.Printf("Request: Delete. Origin: %s", origin)
	resp, err = client.Delete(context.Background(), &pb.FilesystemRequest{Origin: origin})
	tlog.Info.Printf("Response: %v. Error: %v", resp, err)
}
```

Other examples are located in the client_*_test.go test files.


### Create, Delete, Mount and Unmount methods


All methods with filesystem send with simple FilesystemRequest struct with only Origin value and receive simple FilesystemResponse struct with Executed boolean value and Message value.

```go
type FilesystemRequest struct {
	Origin string `protobuf:"bytes,1,opt,name=origin" json:"origin,omitempty"`
}

type FilesystemResponse struct {
	Executed bool   `protobuf:"varint,1,opt,name=executed" json:"executed,omitempty"`
	Message  string `protobuf:"bytes,2,opt,name=message" json:"message,omitempty"`
}
```


### Put method


Put method sends PutRequest struct with Filename, Origin values, and file Content as byte slice and receives PutResponse struct with Executed boolean value and Message value.

```go
type PutRequest struct {
	Filename string `protobuf:"bytes,1,opt,name=filename" json:"filename,omitempty"`
	Content  []byte `protobuf:"bytes,2,opt,name=content,proto3" json:"content,omitempty"`
	Origin   string `protobuf:"bytes,3,opt,name=origin" json:"origin,omitempty"`
}

type PutResponse struct {
	Executed bool   `protobuf:"varint,1,opt,name=executed" json:"executed,omitempty"`
	Message  string `protobuf:"bytes,2,opt,name=message" json:"message,omitempty"`
}
```


### Get method


Get method sends GetRequest struct with Filename and Origin values and receives GetResponse struct with Executed boolean value, Message value and file Content as byte slice.

```go
type GetRequest struct {
	Filename string `protobuf:"bytes,1,opt,name=filename" json:"filename,omitempty"`
	Origin   string `protobuf:"bytes,2,opt,name=origin" json:"origin,omitempty"`
}

type GetResponse struct {
	Executed bool   `protobuf:"varint,1,opt,name=executed" json:"executed,omitempty"`
	Message  string `protobuf:"bytes,2,opt,name=message" json:"message,omitempty"`
	Content  []byte `protobuf:"bytes,3,opt,name=content,proto3" json:"content,omitempty"`
}
```



## REST API



REST Service is a list on port 13000: `localhost:13000`

### Create bucket ORIGIN

```
curl -X POST localhost:13000/buckets -d '{"data":{"origin":"ORIGIN"}}'
```

### Delete bucket ORIGIN

```
curl -X DELETE localhost:13000/buckets/ORIGIN
```

### Mount bucket ORIGIN

```
curl -X POST localhost:13000/buckets/ORIGIN/mount
```

### Unmount bucket ORIGIN

```
curl -X POST localhost:13000/buckets/ORIGIN/unmount
```

### Put file /PATH/FILE to bucket ORIGIN

```
curl -F "filename=@/PATH/FILE" -X POST localhost:13000/buckets/ORIGIN/putfile
```

### Get file FILE from bucket ORIGIN

```
curl -X GET localhost:13000/buckets/ORIGIN/files/FILE --output /PATH/FILE
```

### Remove file FILE from bucket ORIGIN

```
curl -X DELETE localhost:13000/buckets/ORIGIN/files/FILE
```







# Desktop App (Ghost Data Protocol)

## Install

* **Note: requires a node version >= 7 and an npm version >= 4.**
* **Project is based on [electron-react-bolerlate](https://github.com/chentsulin/electron-react-boilerplate). So if you have installation or compilation issues with this project, please see [debugging guide](https://github.com/chentsulin/electron-react-boilerplate/issues/400).**

First, clone the repo via git and then install dependencies with npm.

```bash
$ cd wize-desktop
$ npm install
```

## Run

Start the app in the `dev` environment. This starts the renderer process in [**hot-module-replacement**](https://webpack.js.org/guides/hmr-react/) mode and starts a webpack dev server that sends hot updates to the renderer process:

```bash
$ npm run dev
```

Alternatively, you can run the renderer and main processes separately. This way, you can restart one process without waiting for the other. Run these two commands **simultaneously** in different console tabs:

```bash
$ npm run start-renderer-dev
$ npm run start-main-dev
```

## Packaging

To package apps for the local platform:

```bash
$ npm run package
```

To package apps for all platforms:

First, refer to [Multi Platform Build](https://www.electron.build/multi-platform-build) for dependencies.

Then,
```bash
$ npm run package-all
```

To package apps with options:

```bash
$ npm run package -- --[option]
```

To run End-to-End Test

```bash
$ npm run build
$ npm run test-e2e
```

:bulb: You can debug your production build with devtools by simply setting the `DEBUG_PROD` env variable:
```bash
DEBUG_PROD=true npm run package
```

## How to add modules to the project

You will need to add other modules to this boilerplate, depending on the requirements of your project. For example, you may want to add [node-postgres](https://github.com/brianc/node-postgres) to communicate with PostgreSQL database, or 
[material-ui](http://www.material-ui.com/) to reuse react UI components.

⚠️ Please read the following section before installing any dependencies ⚠️

### Module Structure

This boilerplate uses a [two package.json structure](https://github.com/electron-userland/electron-builder/wiki/Two-package.json-Structure). This means you will have two `package.json` files.

1. `./package.json` in the root of your project
1. `./app/package.json` inside `app` folder

### Which `package.json` file to use

**Rule of thumb** is: all modules go into `./package.json` except native modules. Native modules go into `./app/package.json`.

1. If the module is native to a platform (like node-postgres) or otherwise should be included with the published package (i.e. bcrypt, openbci), it should be listed under `dependencies` in `./app/package.json`.
2. If a module is `import`ed by another module, include it in `dependencies` in `./package.json`.   See [this ESLint rule](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-extraneous-dependencies.md). Examples of such modules are `material-ui`, `redux-form`, and `moment`.
3. Otherwise, modules used for building, testing and debugging should be included in `devDependencies` in `./package.json`.

### Further Readings

See the wiki page, [Module Structure — Two package.json Structure](https://github.com/chentsulin/electron-react-boilerplate/wiki/Module-Structure----Two-package.json-Structure) to understand what is a native module, the rationale behind two package.json structure and more.

For an example app that uses this boilerplate and packages native dependencies, see [erb-sqlite-example](https://github.com/amilajack/erb-sqlite-example).

## CSS Modules

This boilerplate is configured to use [css-modules](https://github.com/css-modules/css-modules) out of the box.

All `.css` file extensions will use css-modules unless it has `.global.css`.

If you need global styles, stylesheets with `.global.css` will not go through the
css-modules loader. e.g. `app.global.css`

If you want to import global css libraries (like `bootstrap`), you can just write the following code in `.global.css`:

```css
@import "~bootstrap/dist/css/bootstrap.css";
```

## Sass support

If you want to use Sass in your app, you only need to import `.sass` files instead of `.css` once:
```js
import './app.global.scss';
```

## Static Type Checking
This project comes with Flow support out of the box! You can annotate your code with types, [get Flow errors as ESLint errors](https://github.com/amilajack/eslint-plugin-flowtype-errors), and get [type errors during runtime](https://github.com/codemix/flow-runtime) during development. Types are completely optional.

## Dispatching redux actions from the main process

See [#118](https://github.com/chentsulin/electron-react-boilerplate/issues/118) and [#108](https://github.com/chentsulin/electron-react-boilerplate/issues/108)

## How to keep your project updated with the boilerplate

If your application is a fork from this repo, you can add this repo to another git remote:

```sh
git remote add upstream https://github.com/chentsulin/electron-react-boilerplate.git
```

Then, use git to merge some latest commits:

```sh
git pull upstream master
```






# RAFT

hraftd is a reference example use of the Hashicorp Raft implementation v1.0. Raft is a distributed consensus protocol, meaning its purpose is to ensure that a set of nodes -- a cluster -- agree on the state of some arbitrary state machine, even when nodes are vulnerable to failure and network partitions. A distributed consensus is a fundamental concept when it comes to building fault-tolerant systems.

A simple example system like hraftd makes it easy to study the Raft consensus protocol in general and Hashicorp's Raft implementation in particular. It can be run on Linux, OSX, and Windows.

## Reading and writing keys
The reference implementation is a very simple in-memory key-value store. You can set a key by sending a request to the HTTP bind address (which defaults to localhost:11000):

```
curl -XPOST localhost:11000/key -d '{"foo": "bar"}'
```
You can read the value for a key like so:

```
curl -XGET localhost:11000/key/foo
```
## Running hraftd
Building hraftd requires Go 1.9 or later. gvm is a great tool for installing and managing your versions of Go.

Starting and running a hraftd cluster is easy. Download hraftd like so:

```
mkdir hraftd
cd hraftd/
export GOPATH=$PWD
go get github.com/otoolep/hraftd
```
Run your first hraftd node like so:
```
$GOPATH/bin/hraftd -id node0 ~/node0
```
You can now set a key and read its value back:
```
curl -XPOST localhost:11000/key -d '{"user1": "batman"}'
curl -XGET localhost:11000/key/user1
```
## Bring up a cluster
A walkthrough of setting up a more realistic cluster is here.

Let's bring up 2 more nodes, so we have a 3-node cluster. That way we can tolerate the failure of 1 node:
```
$GOPATH/bin/hraftd -id node1 -haddr :11001 -raddr :12001 -join :11000 ~/node1
$GOPATH/bin/hraftd -id node2 -haddr :11002 -raddr :12002 -join :11000 ~/node2
```
This example shows each hraftd node running on the same host, so each node must listen on different ports. This would not be necessary if each node running on a different host.

This tells each new node to join the existing node. Once joined, each node now knows about the key:
```
curl -XGET localhost:11000/key/user1
curl -XGET localhost:11001/key/user1
curl -XGET localhost:11002/key/user1
```
Furthermore, you can add a second key:
```
curl -XPOST localhost:11000/key -d '{"user2": "robin"}'
```
Confirm that the new key has been set like so:
```
curl -XGET localhost:11000/key/user2
curl -XGET localhost:11001/key/user2
curl -XGET localhost:11002/key/user2
```
### Stale reads

Because any node will answer a GET request, and nodes may "fall behind" updates, stale reads are possible. Again, hraftd is a simple program, for the purpose of demonstrating a distributed key-value store. If you are particularly interested in learning more about issue, you should check out rqlite. rqlite allows the client to control read consistency, allowing the client to trade off read-responsiveness and correctness.

Read-consistency support could be ported to hraftd if necessary.

## Tolerating failure

Kill the leader process and watch one of the other nodes be elected leader. The keys are still available for query on the other nodes, and you can set keys on the new leader. Furthermore, when the first node is restarted, it will rejoin the cluster and learn about any updates that occurred while it was down.

A 3-node cluster can tolerate the failure of a single node, but a 5-node cluster can tolerate the failure of two nodes. But 5-node clusters require that the leader contact a larger number of nodes before any change e.g. setting a key's value, can be considered committed.

## Leader-forwarding

Automatically forwarding requests to set keys to the current leader is not implemented. The client must always send requests to change a key to the leader or an error will be returned.



# Using WizeBit network in conjunction with the ERC721 protocol

This protocol implies the provision of a guaranteed right to use a graphic, multimedia information to any person while not providing any tool to actually close the information file for any use by third parties. Due to its specific nature of development and architecture, the WizeBit software and network complex has all the necessary capabilities to provide the ERC721 audience with the necessary set of tools for manipulating access rights to information as a guaranteed owner. Access rights can be very diverse:

- Full access to content;
- Partial access to content;
- Full access for a limited period of time;
- Full access to a limited number of people, etc.

Also, to date, ERC721 absolutely does not exclude the possibility of substituting files or file content for a hash belonging to one or another user owning a hash of the file.
Thus WizeBit has all the necessary facilities to provide the basic tools for guaranteed ownership of the file as an unquestionable right to own information. 

Namely:
* WizeBit ensures that the specified hash stored in the ERC721 account can indicate and grant ownership to the only existing file, and any modification will result in the owner losing the rights to the specified file;
* WizeBit provides the necessary tool to restrict the rights of access to the file by third parties;
* WizeBit guarantees the owner of the file its full concealment, integrity, and also anonymity if necessary;

Given the fact that the standard ERC721 is currently a global brand it gives the opportunity to believe that representing of this functionality on the part of WizeBit guarantees a sufficiently high competitiveness and commercial benefits.

### Using WizeBit as an alternative WEB network

Today, an ever-increasing problem of the WEB network is the restriction of information dissemination by regulators, censorship, restriction of anonymity and freedom of expression.

The developed software and network complex WizeBit has in its architectural basis all the necessary capabilities to provide the end user with services that solve the above-mentioned shortcomings of the existing WEB network. To solve the described problem, developers need to develop 2 basic tools that allow emulating websites inside the WizeBit network. It is necessary to develop:

* the standard for storing files when accessing to which the available content analog of the WEB page will be formed;
* web browser on the basis of an existing desktop application for viewing the above-described web pages;
* routing protocol between the WIB application pages;
* nodes for storing addresses of WIB sites located inside the WizeBit network;


# Audio / Video / Chat communication systems inside WizeBit

One of the most vital problems of today's messengers is their potential cooperation with regulatory bodies that entails the danger of disclosure of personal data in communication via audio, video, chat.

WizeBit network is positioned as an absolutely anonymous and confidential channel for data exchange (the guarantee is based on the use of technical and architectural solutions based on the use of data encryption algorithms SHA, AES, RSA) and gives every reason to believe that this complex can be used in the sphere of communication. The completion of the complex will take minor time consumption.

To popularize the "messenger" on the basis of the WizeBit network, it is necessary to prepare a brief presenting the architecture proving that the exchange of text messages between users is absolutely safe and confidential and that the text of messages is inaccessible even for the WizeBit developers themselves.

Based on the foregoing, we can summarize that the first stage of developing a messenger based on the WizeBit network is the implementation of text messaging between users. If the first stage is successfully implemented, it is possible to start the exchange of audio/video streams between the end users of the WizeBit network.
