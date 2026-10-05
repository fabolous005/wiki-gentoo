<!-- source: https://wiki.gentoo.org/wiki/Monero | group: Gentoo Wiki (Main) | wiki-title: Monero -->
---
title: Monero
url: https://wiki.gentoo.org/wiki/Monero
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-12"
fingerprint: b42f118b2fa73bcc
license: CC BY-SA 4.0
---

# Monero

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Monero** is a privacy and security-focused cryptocurrency originating from the 2013 CryptoNote protocol.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> It uses a combination of ring signatures<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> and zero-knowledge proofs to implement "Ring Confidential Transactions" (RingCT)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> intended to obfuscate the sender, receiver, and amount of a transaction.[\[4\]](https://wiki.gentoo.org#cite_note-4)

As these privacy features prevent the use of Bitcoin's [SPV (Simplified-Payment-Verification)](https://en.bitcoin.it/wiki/Scalability#Simplified_payment_verification), it is necessary to distribute the software in two separate programs: the wallet and the node. The **Wallet** (Client) software has control over funds and is the minimum needed to make a private transaction and accept payments. The wallet connects to a **Node** (Server) that may run on the same computer or on another computer over the network. The Node connects to the peer-to-peer network, stores and relays transactions, and works together with with other computers on the network to come to a consensus about the order in which they occur.

Cryptocurrencies like Bitcoin use [Proof-of-Work](https://en.wikipedia.org/wiki/Proof_of_work) to make it difficult for an attacker to reverse the order of transactions (which would allow the attacker to [spend funds more than once](https://en.wikipedia.org/wiki/Double-spending)). Unlike Bitcoin, Monero uses a Proof-of-Work system which is designed to mitigate the effectiveness of specialized hardware (ASICs) as opposed to general-purpose CPUs<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>. Because of this, it becomes necessary to distribute **Mining Software** for everyday users that may wish to secure the network with their own hardware. This article discusses methods for mitigating [cryptojacking](https://en.wikipedia.org/wiki/Cryptojacking) attacks.

## Wallet Software

### Official

The Monero Core team develops the official wallet in CLI & GUI variants.
More information about them can be found on the [project's download page](https://www.getmonero.org/downloads/).

#### Monero CLI

The command-line wallet can be installed by emerging `net-p2p/monero[wallet-cli]`.

It can be summoned with:

`user $``monero-wallet-cli`
#### Monero GUI

The Graphical wallet is available as a binary package:

`root #``emerge --ask net-p2p/monero-gui-bin`
.

The wallet has a Simple and an Advanced mode. Using the former might be a good idea if you are just starting.

### Third party

#### Feather

[Feather Wallet](https://featherwallet.org/) is a lightweight and simple Qt5/Qt6 implementation of the monero wallet.

![](https://wiki.gentoo.org/images/thumb/a/a0/Feather_wallet.png/300px-Feather_wallet.png)

`root #``emerge --ask net-p2p/feather`
By default, it's configured to connect to a remote node over the internet (and to onion sites if the [tor daemon](https://wiki.gentoo.org/wiki/Tor) is running).

To connect feather wallet to a local node instead, select File>Settings>Network>Node>Add custom node(s) and add 127.0.0.1:18081 to the custom nodes list.

## Node software

### monerod

monerod is the only "official" node implementation.

#### Installation

`root #``emerge --ask net-p2p/monero`
#### Configuration

Because monero's blockchain (the list of transactions, also called the "ledger") can take up quite a lot of space (about 164 GB as of 2025<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>) The two most important configuration settings are the directory in which it is stored (/var/lib/monero by default), and whether or not it should be "pruned". Pruning is an option that reduces storage usage by about 60%, storing an incomplete but usable copy of the blockchain<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup>. Below is an example configuration file at /etc/monero/monerod.conf that changes both of these options:

**`/etc/monero/monerod.conf`**

**Setting blockchain directory and pruning**

```
=/your/monero/directory/
log-file=/your/monero/directory/monero.log
prune-blockchain=1
```
If you have a publicly available node, you may want to enable restricted RPC:

**`/etc/monero/monerod.conf`**

**Enabling restricted RPC**

```
=1
```
If you send transactions from this node, you may also want to route your transactions over the Tor network instead of relying on Dandelion++. Install and configure  [Tor](https://wiki.gentoo.org/wiki/Tor), then add the following to your monerod configuration:

**`/etc/monero/monerod.conf`**

**Routing transactions over Tor on localhost:9050**

```
=tor,127.0.0.1:9050,12,disable_noise
```
See [monerod options](https://docs.getmonero.org/interacting/monerod-reference/) and [monerod.conf](https://docs.getmonero.org/interacting/monero-config-file/) for a more complete list of configuration options.

#### Service

To start immediately:

`root #``rc-service monerod start`
To start the monerod service on system boot, add it to the default runlevel:

`root #``rc-update add monerod default`
To get monerod's current status:

`user $``monerod status`
or

`user $``rc-service monerod status`
### Cuprate

[Cuprate](https://github.com/Cuprate/cuprate) is an alternate implementation of the monero server software with a focus on performance and security. Its development is intended to improve node diversity and resilience of the Monero network as whole against implementation vulnerabilities (side-channel attacks). It is currently under development and requires a nightly build of rust to compile.

You can still compile it yourself out-of-tree by cloning the [the repo](https://github.com/Cuprate/cuprate) and running `cargo build` if you use a rustup-installed nightly toolchain in your home directory as described in [Rust#Rustup](https://wiki.gentoo.org/wiki/Rust#Rustup).

## Mining Software

To incentivize users to secure the network with Proof-of-Work, 0.6 XMR is randomly rewarded every 2 minutes to a successful miner<sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup>. The average expected return can be calculated by multiplying the [expected hashrate](https://xmrig.com/benchmark), dividing by the total network hashrate, then subtracting the total cost of electricity consumed. See the [monero mining calculator](https://www.monero.how/monero-mining-calculator). Generally speaking, Monero mining is not profitable without access to relatively modern CPUs and cheap electricity, or use of computers for indoor heating. GPU mining is also possible, but it is far less profitable compared to other cryptocurrencies.

While monerod does support mining by itself, it's recommended to use a faster mining implementation instead of monerod's built-in mining.

### XMRig

XMRig is the largest fast implementation of Monero mining. This gentoo package respects the `opencl` USE flag for AMD GPUs, however the use of Nvidia GPUs through CUDA requires a [separate plugin](https://github.com/xmrig/xmrig-cuda).

#### Installation


`root #``emerge --ask net-misc/xmrig`
#### Usage

If a monero node is running, (localhost on port 18081), solo mining is as simple as

`user $``xmrig -u YOUR_MONERO_ADDRESS` You may want to use ZMQ instead of RPC for mining. Because ZMQ is lower latency it will improve your mining profitiability:

**`/etc/monero/monerod.conf`**

**Enabling zmq on monerod for mining**

```
=tcp://127.0.0.1:18083
```
Another option is pool mining, which eliminates the need to run a monero node, and may offer more regular payouts at the cost of increased centralization and fees. Configuration depends on the pool, but usually looks something like:

`user $``xmrig -o your-mining-pool.com:port -u YOUR_MONERO_ADDRESS` #### Optimization

Disabling "hardware prefetchers" has been shown to increase mining performance by about 30%. This can be done by using the wrmsr command as shown in /usr/bin/randomx\_boost.sh. XMRig automatically runs this script when run as root.

Enabling "1GB huge pages" also yields a slight performance improvement on linux systems, at the cost of using more memory. This is done through the /usr/bin/enable\_1gb\_pages.sh script.

See also the [RandomX Optimization Guide](https://xmrig.com/docs/miner/randomx-optimization-guide)

### P2Pool

P2Pool is a decentralized method of spreading out mining rewards into regular payouts, similar to a regular mining pool but with lower fees. The P2Pool software connects to a monero node and exposes a pool that miners such as XMRig can connect to.

#### Installation

`root #``emerge --ask net-p2p/p2pool`
#### Usage

p2pool can be started in the current directory by running

`user $``p2pool --host MONERO_NODE_ADDRESS --wallet YOUR_PRIMARY_ADDRESS`
Where `MONERO_NODE_ADDRESS` is the address of the monero node (for example, 127.0.0.1 for a monero node that runs on the same computer), and `YOUR_PRIMARY_ADDRESS` is a monero primary address (begins with a "4" as opposed to a secondary address that begins with an "8") that will receive the mining reward. Once the P2Pool software finishes connecting to the network, one can start mining with XMRig as if it's a normal pool. By default, this is exposed on port 3333:

`user $``xmrig -o 127.0.0.1:3333`
There are currently two different networks: p2pool and p2pool-mini. If a computer has a lower hashrate, it may be better to connect to the p2pool-mini network instead by passing the --mini flag to p2pool:

`user $``p2pool --mini --host MONERO_NODE_ADDRESS --wallet YOUR_PRIMARY_ADDRESS`
See also [P2Pool FAQ](https://p2pool.io/#faq)

## Troubleshooting

### monerod can't sync

Sometimes monerod can get stuck if the computer is forcibly shutdown (for example, electrical outage or running out of battery). This can usually be fixed with monerod --db-salvage.

## Cryptojacking Prevention

Cryptojacking is the act of hijacking a computer to mine cryptocurrency against the owner's will. Because of the CPU-bound nature of Monero's Proof-of-Work system (RandomX), it's easier for a hacker to capitalize on a victim's computing power relative to the rest of the network. However, RandomX also has some features designed to mitigate cryptojacking.

It is often speculated that a large portion of Monero's hashrate is a result of cryptojacked "IoT" devices (appliances such as cameras, toasters, etc. that contain computers which are unnecessarily connected to the internet). It is important to consider RandomX's [Memory-hardness](https://en.wikipedia.org/wiki/Memory-hard_function) on low-end devices, which prevents it from running at all on computers with less than 256 MiB of memory, and massively decreases mining efficiency on devices with less than 2080 MiB.

Cryptojacking can often be used to extract additional money from a hacked VPS service at great cost to the client. One feature of RandomX is that efficient miners will necessarily leave a detectable trace in the state of CPU registers. ["RandomX Sniffer"](https://github.com/tevador/randomx-sniffer) is a proof-of-concept tool that can be used to detect cryptojacking malware. In theory, VPS providers could use this method to warn clients or prevent cryptojacking though the use of [Virtual Machine Introspection](https://en.wikipedia.org/wiki/Virtual_machine_introspection). If the client has a firewall installed, then they can also just monitor outbound connections to common Monero pools or to the P2Pool network.

Lastly, a common another attack vector is web mining (sometimes referred to as "Drive-by Mining"), which involves the insertion of mining scripts into webpages, taking advantage of the computers that access the web page. Web miners are particularly inefficient because of the decrease in branch prediction speeds and the lack of directed rounding support for floating point operations in JavaScript and WebAssembly. As such, web mining is typically orders of magnitude less efficient than mining natively. When hosting a web service, pay attention to securing the website against [cross-site scripting attacks](https://en.wikipedia.org/wiki/Cross-site_scripting). As a user there is not much that can be done to avoid web mining besides installing an ad-blocker or disabling JavaScript altogether. Some browsers like [Firefox](https://wiki.gentoo.org/wiki/Firefox) have default settings to block common crypto mining scripts. It's also important to consider [Web extensions](https://blog.chromium.org/2018/04/protecting-users-from-extension-cryptojacking.html) as another vector for web mining.

## Libraries

[Monero-rs](https://github.com/monero-rs/monero-rs) is a similar monero library for the Rust programming language.

[MoneroPy](https://github.com/bigreddmachine/moneropy) is a demonstration of some programming concepts in monero. See also [MiniNero](https://github.com/monero-project/mininero) and the [Zero to Monero](https://www.getmonero.org/library/Zero-to-Monero-2-0-0.pdf) tutorial.

## External resources

- N. Alsalami and B. Zhang, [SoK: A Systematic Study of Anonymity in Cryptocurrencies](https://ieeexplore.ieee.org/document/8937681), 2019 IEEE Conference on Dependable and Secure Computing (DSC), Hangzhou, China, 2019, pp. 1-9, doi: 10.1109/DSC47296.2019.8937681.
- RandomX proof-of-work (PoW) algorithm — [https://github.com/tevador/randomx](https://github.com/tevador/randomx)
- Shen Noether, [Ring Signature Confidential Transactions for Monero](https://eprint.iacr.org/2015/1098), 2015 Cryptology ePrint Archive, Paper 2015/1098.
