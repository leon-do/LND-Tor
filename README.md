# How to Run Tor with LND

## Install Tor
  [Linux](https://www.torproject.org/docs/debian.html.en)
  
    $ sudo apt install tor

  [OSX](https://www.torproject.org/docs/tor-doc-osx.html.en)
        
    $ brew install tor

## Verify installation
  ``` 
  $ tor --version
  ```

## Add Tor Configuration to `torrc`

  Linux: `/etc/tor/torrc`

  OSX: `/usr/local/etc/tor/torrc` 
  
  ```
  SOCKSPort 9050
  Log notice stdout
  ControlPort 9051
  CookieAuthentication 1
  ```

## Run Tor
    $ tor

## Run lnd 

  Enable lnd to connect to Tor
  - `tor.active` allows lnd to route through Tor
  - `tor.v3` sets up a v3 onion service
  - `tor.streamisolation` will create a new circuit for each connection
  - `listen` to localhost to prevent unintentional leaking of identifying information

  ```
  $ lnd --tor.active --tor.v3 --listen=localhost --tor.streamisolation
  ```

  or update your `lnd.conf`

  ```
  tor.active=true
  tor.v3=true
  tor.streamisolation=true
  listen=localhost
  ```
  
## Verify lnd node info
  
  Get your public key
  ```
  $ lncli getinfo | grep identity_pubkey

  "identity_pubkey": "0346095e50ed1f8cf4dbda1fca442cd2ebccf082912e33c1c2e19868f1f56a190a",
  ```

  Get node information about your public key
  ```
  $ lncli getnodeinfo 0346095e50ed1f8cf4dbda1fca442cd2ebccf082912e33c1c2e19868f1f56a190a

  {
    "node": {
        "last_update": 1548783346,
        "pub_key": "0346095e50ed1f8cf4dbda1fca442cd2ebccf082912e33c1c2e19868f1f56a190a",
        "alias": "0346095e50ed1f8cf4db",
        "addresses": [
            {
                "network": "tcp",
                "addr": "b53ztxul4vdcktgcgmvcvgjigi2vq2hy4ah6wg7frqpiiesdoxozx3ad.onion:9735"
            }
        ],
        "color": "#3399ff"
    },
    "num_channels": 7,
    "total_capacity": "11732911"
  }
  ```

  Verify that your `addr` is an `onion` address

  or if you have Zap installed

  ![](/img/01.png)

## How to Connect 

Connecting to an Tor node is the same as connecting to any other node: `publicKey@address:port`

Example:
```
lncli openchannel --node_key 0346095e50ed1f8cf4dbda1fca442cd2ebccf082912e33c1c2e19868f1f56a190a --connect b53ztxul4vdcktgcgmvcvgjigi2vq2hy4ah6wg7frqpiiesdoxozx3ad.onion:9735 --local_amt 20000 
```

or connect manually through Zap

  ![](/img/02.png)
  ![](/img/03.png)
  ![](/img/04.png)

## Reference 

https://github.com/lightningnetwork/lnd/blob/master/docs/configuring_tor.md
