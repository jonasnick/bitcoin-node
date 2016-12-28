03 Blockchain and Mempool
===

Blocks
---
* `GENESIS=`bc getblockhash 0` `
* `bc getblock $GENESIS | egrep "confirmations|tx|\"size\"|height|\"time\""`
* `bc getblock $GENESIS false | xxd -r -p`

Blockchain
---
* `bc getblockchaininfo  | jq ".bip9_softforks"`
* `bc getmininginfo | grep networkhashps`
* "pruning"
    * only save the last n MB of blocks
    * `bd ... -prune=n`

Mempool
---
* mempool consists of known transactions
    not (yet) in a block
* `bc getmempoolinfo`
* `bd ... -blocksonly`
    * no tx relay, no mempool,
        no compact blocks
* `bd ... -minrelaytxfee=0.0001`
    * can prevent gossip spam
* txfee
    * `FEEPERKB=`bc estimatefee 3` `
    * `python -c "print 930 * $FEEPERKB * 1.0/4, 'approx. USD per median tx'"`
