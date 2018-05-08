01 Installation
===

What is Bitcoin?
---
It's the first p2p currency.

* transactions
* P2P
* blocks
* miners
* blockchain

What is Bitcoin Core?
---
* continuation of the work of
    Bitcoin's creator
* what does it do?
    * p2p
    * consensus
    * wallet
    * rpc

Why run a full node?
---
* personal wallet
* merchant
* API

Specific use case
---
* there's a satoshi square
    everyday at 33c3
* want to use most secure and
    private wallet available
* => smartphone + ssh + Bitcoin Core

Where do you run it?
---
* not very powerful machine required
* but not really raspberry pi either
    * ARM: cubox, odroid
* 4GB of RAM
* full blockchain: big harddrive,
    disk possible
* preparing the host (example)
    * full disk encryption
    * set date
    * firewall
    * systemd service

How do you get it?
---
* download binary from bitcoin.org
    * `wget https://bitcoin.org/bin/bitcoin-core-0.16.0/bitcoin-0.16.0-x86_64-linux-gnu.tar.gz`
    * `wget https://bitcoin.org/bin/bitcoin-core-0.16.0/SHA256SUMS.asc`
    * verify signatures with gpg
        * sign my key to get a trust path
    * gitian deterministic builds
        * https://github.com/bitcoin-core/gitian.sigs
* source code releases on github

Starting up bitcoind
---
* bitcoind is the daemon
* connects to peers
* downloads and verifies new blocks
    * initial block download (IBD)
        * 27h on the-donald
        * on reasonably fast hardware < 8h
* relays blocks and transactions

bitcoin-cli
---
* talks to bitcoind using JSON-RPC
    * `alias bc="bitcoin-cli"`
* `bc -getinfo | egrep "\"version\"|balance|blocks|connections|errors"`
* `bc help`
* `bc help sendtoaddress`

bitcoin-qt
---
* GUI
    * ?
    * I don't use such things

Configuration
---
* the .bitcoin folder
    * bitcoin.conf
    * blocks/
    * `cat .bitcoin/debug.log | egrep "receive version message|UpdateTip|Connect total" | tail -n 100`
* `alias bd="bitcoind"`
* arguments
    * `bd ... -help`
    * `bd ... -testnet` and `bc ... -regtest`
        * different p2p network and blockchain
    * `bd ... -debug`
    * `bd ... -dbcache=3000`
        * (for faster IBD)

Resources
---
* https://bitcoin.org/en/developer-documentation
* https://en.bitcoin.it/wiki/
* https://bitcoin.stackexchange.com/
