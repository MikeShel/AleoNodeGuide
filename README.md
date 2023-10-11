# AleoNodeGuide
Aleo Node Guide

## 1 Requirements

The following are **minimum** requirements to run an Aleo node:
 - **OS**: 64-bit architectures only, latest up-to-date for security
    - Clients: Ubuntu 22.04 (LTS), macOS Ventura or later, Windows 11 or later
    - Provers: Ubuntu 22.04 (LTS), macOS Ventura or later
    - Validators: Ubuntu 22.04 (LTS)
 - **CPU**: 64-bit architectures only
    - Clients: 16-cores
    - Provers: 32-cores (64-cores preferred)
    - Validators: 32-cores (64-cores preferred)
 - **RAM**: DDR4 or better
    - Clients: 16GB of memory
    - Provers: 32GB of memory (64GB or larger preferred)
    - Validators: 64GB of memory (128GB or larger preferred)
 - **Storage**: PCIe Gen 3 x4, PCIe Gen 4 x2 NVME SSD, or better
    - Clients: 64GB of disk space
    - Provers: 128GB of disk space
    - Validators: 2TB of disk space (4TB or larger preferred)
 - **Network**: Symmetric, commercial, always-on
    - Clients: 100Mbps of upload **and** download bandwidth
    - Provers: 250Mbps of upload **and** download bandwidth
    - Validators: 500Mbps of upload **and** download bandwidth
- **GPU**:
    - Clients: Not required at this time
    - Provers: CUDA-enabled GPU (optional)
    - Validators: Not required at this time

### 2 Installation

Before beginning, please ensure your machine has `Rust v1.66+` installed. Instructions to [install Rust can be found here.](https://www.rust-lang.org/tools/install)

Start by cloning this Github repository:
```
git clone https://github.com/AleoHQ/snarkOS.git --depth 1
```

Next, move into the `snarkOS` directory:
```
cd snarkOS
```

**[For Ubuntu users]** A helper script to install dependencies is available. From the `snarkOS` directory, run:
```
./build_ubuntu.sh
```

Lastly, install `snarkOS`:
```
cargo install --path .
```

Please ensure ports `4133/tcp` and `3033/tcp` are open on your router and OS firewall.

## 3. Run an Aleo Node

## 3a. Run an Aleo Client

Start by following the instructions in the [Build Guide](#2-build-guide).

Next, to start a client node, from the `snarkOS` directory, run:
```
./run-client.sh
```

## 3b. Run an Aleo Prover

Start by following the instructions in the [Build Guide](#2-build-guide).

Next, generate an Aleo account address:
```
snarkos account new
```
This will output a new Aleo account in the terminal.

**Please remember to save the account private key and view key.** The following is an example output:
```
 Attention - Remember to store this account private key and view key.

  Private Key  APrivateKey1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  <-- Save Me And Use In The Next Step
     View Key  AViewKey1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  <-- Save Me
      Address  aleo1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  <-- Save Me
```

Next, to start a proving node, from the `snarkOS` directory, run:
```
./run-prover.sh
```
When prompted, enter your Aleo private key:
```
Enter the Aleo Prover account private key:
APrivateKey1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## 4. FAQs

### 1. My node is unable to compile.

- Ensure your machine has `Rust v1.66+` installed. Instructions to [install Rust can be found here.](https://www.rust-lang.org/tools/install)
- If large errors appear during compilation, try running `cargo clean`.
- Ensure `snarkOS` is started using `./run-client.sh` or `./run-prover.sh`.

### 2. My node is unable to connect to peers on the network.

- Ensure ports `4133/tcp` and `3033/tcp` are open on your router and OS firewall.
- Ensure `snarkOS` is started using `./run-client.sh` or `./run-prover.sh`.

### 3. I can't generate a new address ### 

- Before running the command above (`snarkos account new`) try `source ~/.bashrc`
- Also double-check the spelling of `snarkos`. Note the directory is `/snarkOS`, the command is `snarkos`

## 5. Command Line Interface

To run a node with custom settings, refer to the full list of options and flags available in the `snarkOS` CLI.

The full list of CLI flags and options can be viewed with `snarkos --help`:
```
snarkOS 
The Aleo Team <hello@aleo.org>

USAGE:
    snarkos [OPTIONS] <SUBCOMMAND>

OPTIONS:
    -h, --help                     Print help information
    -v, --verbosity <VERBOSITY>    Specify the verbosity [options: 0, 1, 2, 3] [default: 2]

SUBCOMMANDS:
    account    Commands to manage Aleo accounts
    clean      Cleans the snarkOS node storage
    help       Print this message or the help of the given subcommand(s)
    start      Starts the snarkOS node
    update     Update snarkOS
```

The following are the options for the `snarkos start` command:
```
USAGE:
    snarkos start [OPTIONS]

OPTIONS:
        --network <NETWORK_ID>                  Specify the network ID of this node [default: 3]
        
        --validator                             Specify this node as a validator
        --prover                                Specify this node as a prover
        --client                                Specify this node as a client
        
        --private-key <PRIVATE_KEY>             Specify the node's account private key
        --private-key-file <PRIVATE_KEY_FILE>   Specify the path to a file containing the node's account private key
        
        --node <IP:PORT>                        Specify the IP address and port for the node server [default: 0.0.0.0:4133]
        --connect <IP:PORT>                     Specify the IP address and port of a peer to connect to
 
        --rest <REST>                           Specify the IP address and port for the REST server [default: 0.0.0.0:3033]
        --norest                                If the flag is set, the node will not initialize the REST server
        
        --nodisplay                             If the flag is set, the node will not render the display
        --verbosity <VERBOSITY_LEVEL>           Specify the verbosity of the node [options: 0, 1, 2, 3] [default: 2]
        --logfile <PATH>                        Specify the path to the file where logs will be stored [default: /tmp/snarkos.log]
        
        --dev <NODE_ID>                         Enables development mode, specify a unique ID for this node
```

## 6. Development Guide

### 6.1 Quick Start

In the first terminal, start the first validator by running:
```
cargo run --release -- start --nodisplay --dev 0 --validator
```
In the second terminal, start the second validator by running:
```
cargo run --release -- start --nodisplay --dev 1 --validator
```
In the third terminal, start the third validator by running:
```
cargo run --release -- start --nodisplay --dev 2 --validator
```
In the fourth terminal, start the fourth validator by running:
```
cargo run --release -- start --nodisplay --dev 3 --validator
```

From here, this procedure can be used to further start up provers and clients.

### 6.2 Operations

It is important to initialize the nodes starting from `0` and incrementing by `1` for each new node.

The following is a list of options to initialize a node (replace `<NODE_ID>` with a number starting from `0`):
```
cargo run --release -- start --nodisplay --dev <NODE_ID> --validator
cargo run --release -- start --nodisplay --dev <NODE_ID> --prover
cargo run --release -- start --nodisplay --dev <NODE_ID> --client
cargo run --release -- start --nodisplay --dev <NODE_ID>
```

When no node type is specified, the node will default to `--client`.

##### Clean Up

To clean up the node storage, run:
```
cargo run --release -- clean --dev <NODE_ID>
```

