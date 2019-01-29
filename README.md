### Install Tor: https://www.torproject.org/projects/torbrowser.html.en
  - Linux
    
        $ sudo apt install tor

  - OSX:
        
        $ brew install tor

### Verify installation
    
    $ tor --version
    Tor version 0.3.5.7.

### Create Tor config
  - Linux: 

        $ vi /etc/tor/torrc

  - OSX: 
  
        $ vi /usr/local/etc/tor/torrc

### Update `torrc` configuration file
  ```
  SOCKSPort 9050
  Log notice stdout
  ControlPort 9051
  CookieAuthentication 1
  ```

### Run Tor
  ```
  $ tor
  Jan 29 10:39:18.561 [notice] Read configuration file "/usr/local/etc/tor/torrc".`
  ```

### Run lnd
  ```
  $ ./lnd --tor.active --tor.v2 --tor.streamisolation

  2019-01-29 10:28:33.353 [INF] SRVR: Proxying all network traffic via Tor (stream_isolation=true)! NOTE: Ensure the backend node is proxying over Tor as well
  ```

  or update your `lnd.conf`

  ```
  #  Allows lnd to route all outbound and inbound connections through Tor.
  tor.active=true
  #  Tor can simply be done with either v2 or v3 onion services
  tor.v3=true
  # Optional: Enable Tor stream isolation by randomizing user credentials for each connection
  tor.streamisolation=true
  ```

### Verify lnd node info
  ```
  $ ./lnd getinfo | grep identity_pubkey

  "identity_pubkey": "0346095e50ed1f8cf4dbda1fca442cd2ebccf082912e33c1c2e19868f1f56a190a",

  $ ./lnd getnodeinfo 0346095e50ed1f8cf4dbda1fca442cd2ebccf082912e33c1c2e19868f1f56a190a

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

  address is now `b53ztxul4vdcktgcgmvcvgjigi2vq2hy4ah6wg7frqpiiesdoxozx3ad.onion:9735`

  fin

### Reference https://github.com/lightningnetwork/lnd/blob/master/docs/configuring_tor.md
