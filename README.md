# Ghost Drive Blockchain 


Currently, Wise software development is carried out in the following areas:

### Blockchain | Storage | Desktop App | Raft | Web

Implementation of the phased development of a private (closed) BlockChain network that allows to host nodes on the main types of servers (dedicated servers, cloud storage, VPS, etc.). A node as the main entity of a Blockchain network must be equipped with a basic software and mathematical tools technique to participate in all the necessary BlockChain processes, namely:

* automatic self-determination of the node in the network;
* adding transactions;
* implementation of consensus;
* support of the mining process.

Wise Soft network have a software interface for desktop programs that allows to save files within the network while observing a number of security measures. The main purpose of the Wise Blockchain network to be developed is to ensure the maximum security, confidentiality and invulnerability of the information transferred to the storage. 

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

# Network
### Network nodes and their roles

### We’ll have three node roles:

### The central node (Master). 
This is the node all other nodes will connect to, and this is the node that’ll send data between other nodes.

### A miner node.
This node will store new transactions in mempool and when there’re enough of transactions, it’ll mine a new block.

### A wallet node. 
This node will be used to send coins between wallets. It’ll store a full copy of blockchain.



# Wize Protocol
### Nodes communicate by the means of messages.

When a new node is run mode, it gets several nodes from a DNS seed, and sends them version message. If node is not the central one, it must send version message to the central node to find out if its blockchain is outdated.

Next message getblocks means “show me what blocks you have” (in Bitcoin, it’s more complex). Pay attention, it doesn’t say “give me all your blocks”, instead it requests a list of block hashes.

WizeBit uses inv to show other nodes what blocks or transactions current node has. Again, it doesn’t contain whole blocks and transactions, just their hashes.

Message getdata is a request for certain block or transaction, and it can contain only one block or transaction ID. The handler is straightforward: if they request a block, return the block; if they request a transaction, return the transaction. Notice, that we don’t check if we actually have this block or transaction.
Messages block and tx actually transfer the data.

Wize Protocol also will be able to communicate to IoT devices in own encryption protocol. 

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


# Wize Wallet

In WizeBlock, your identity is a pair of private and public keys stored on your computer (or stored in some other place you have access to). A wallet is nothing but a key pair. In the construction of wallet a new key pair is generated with ECDSA which is based on elliptic curves. A private key is generated using the curve, and a public key is generated from the private key. One thing to notice: in elliptic curve based algorithms, public keys are points on a curve. This is a public key is a combination of X, Y coordinates.

If you want to send coins to someone, you need to know their address. But addresses (despite being unique) are not something that identifies you as the owner of a “wallet”. In fact, such addresses are a human-readable representation of public keys. The address generation algorithm utilizes a combination of open algorithms that take a public key and returns real Base58-based address.

Currently, WizeBlock has generating wallets on the WizeBlock node side, but in the next version wallets will generate on the user side in the Desktop application.

# Mining

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
