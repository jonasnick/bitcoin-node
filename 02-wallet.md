02 Wallet
===

Example: Buy bitcoins
---
* get new address and print QR code
```
ADDR=`bc getnewaddress`
qrencode -m 1 -t ansi256 bitcoin:$ADDR
```

* check if coins are received
```
bc getreceivedbyaddress $ADDR 0
bc getreceivedbyaddress $ADDR 1
```

* polling is not the smartest way to do this
    * `bd ... -walletnotify='echo %s >> ~/wallet.txt'`
    * `bd ... -zmqpubrawblock=<zmq socket address>`

Example: Sell bitcoins
---
```
bc sendtoaddress <address> <amount>
```

Transaction info
---
* `bc listtransactions`
* `bc gettransaction <txid> | egrep "confirmation|fee" `
* list your addresses
    *  `bc getaddressesbyaccount ""`

Backup
---
* `~/.bitcoin/wallet.dat` contains
    * master seed
    * addr and tx metadata
* backup: turn off bitcoind and copy wallet.dat
* restore: copy saved wallet.dat back
    * start with `bd ... -rescan`
    * `bc getnewaddress`
* encryption
    * either `bc encryptwallet`
        * metadata not encrypted
    * or manual

Offline signing
---
* means that signing key is not on machine
  connected to the internet
* with Bitcoin Core
    * https://people.xiph.org/~greg/signdemo.txt
    * https://github.com/rustyrussell/bitcoin-storage-guide
* or hardware wallets
    * ledger
    * trezor
    * digital bitbox

Transactions
---
* `bc getrawtransaction`
    * requires `bd ... -txindex`
    * `bc getrawtransaction f4184fc596403b9d638783cf57adfe4c75c605f6356fbc91338530e9831e9e16 1`
    * can't look up "addresses", as there is
      no index for it

* an exemplary transaction subgraph
    ```
    +------------+
    |       out0 +---+
    +------------+   |    +------------+
    TxA              +----> in0   out0 |
                          |            |
                     +----> in1        |
    +------------+   |    +------------+
    |       out0 +---+    TxC
    |            |
    |       out1 |
    +------------+
    TxB
    ```

"Raw" transaction interface
---
* useful for
    * spending multisig
        * multisig: f.e. need 2 of 3 keys to
          spend
        * creating
          `bc createmultisig 1 '["$PK1", "$PK2"]'`
    * offline signing
    * control over which exact coins
      to spend

* `bc listunspent`
* `bc createrawtransaction`
    * don't forget to add change!
* `bc signrawtransaction`
* `bc submitrawtransaction`

* even more control with the bitcoin-tx tool

Create m-of-n multisig
---
* `bc getnewaddress`
* find pubkey with `bc validateaddress <address>` and broadcast to signers
* all signers: `bc addmultisigaddress m '["pubKey_1", ..., "pubKey_n"]'`

Spend m-of-n multisig
---
* `bc createrawtransaction '[{"txid": "multisig_output_txid",  "vout": vout, "nValue": value}]' '{"destination_address": value}'`
* `bc signrawtransaction <txhex>`
* pass signature to next signer until complete
* `bc sendrawtransaction <signed_txhex>`

