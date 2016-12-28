04 Network
===

Network
---
* `bc getpeerinfo | egrep 'subver|inbound'`
* `bc addnode`
* `bc setban`
    * https://0bin.net/paste/ZFEAihJLl8AQRaBx#M8xZGuMyYKZWhjfNLOX4pQsraQys2IedFPr60rB3rJi

Setting Network Limits
---
* check bandwidth usage
    * `vnstat -d`
* `bd ... -listen=0`
* `bd ... -maxconnections=20`
* `bd ... -maxuploadtarget=500`
* `bd ... -blocksonly`


Tor
---
* https://github.com/bitcoin/bitcoin/blob/master/doc/tor.md
* install tor
* run behind Tor proxy
    * `bd ... -proxy=127.0.0.1:9050`

Run a bitcoin hidden service
---
* add to torrc
```
HiddenServiceDir /var/lib/tor/bitcoin-service/
HiddenServicePort 8333 127.0.0.1:8333
```

* `ONION=`cat /var/lib/tor/bitcoin-service/hostname` `
* `bd ... -proxy=127.0.0.1:9050`
* `bd ... -listen=1`
    * listen despite proxy
* `bd ... -listenonion=0`
    * no ephemeral HS
* `bd ... -externalip=$ONION`
* `bd ... -bind=127.0.0.1`
    * otherwise still allowing non-Tor

* `bc getnetworkinfo`

Ephemeral Hidden services
---
* configure tor control port
