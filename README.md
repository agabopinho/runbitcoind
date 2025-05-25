# Bitcoin Core Node with Tor

This project sets up a Bitcoin Core full node that routes all traffic through the Tor network for enhanced privacy and anonymity.

## Overview

This Docker Compose configuration creates two containers:
- A Tor proxy service to provide anonymous networking
- A Bitcoin Core node that connects exclusively through the Tor network

## Prerequisites

- Docker and Docker Compose installed on your system
- Sufficient disk space for the Bitcoin blockchain (>500GB recommended)
- Open ports on your firewall/router (if needed)

## Setup

This project includes two configurations:

### 1. Initial Blockchain Sync (Fast Download)
Use `docker-compose-sync.yml` for the initial blockchain download:

1. Clone this repository
2. Make sure you have the directory `D:/bitcoind` created (or modify the volume path)
3. Start the fast sync configuration:
   ```bash
   docker-compose -f docker-compose-sync.yml up -d
   ```
4. Monitor the sync progress:
   ```bash
   docker-compose -f docker-compose-sync.yml logs -f
   ```

### 2. Tor Configuration (After Sync)
Once the blockchain is fully synced, switch to the Tor configuration for privacy:

1. Stop the sync configuration:
   ```bash
   docker-compose -f docker-compose-sync.yml down
   ```
2. Start the Tor configuration:
   ```bash
   docker-compose -f docker-compose-tor.yml up -d
   ```
3. Check the logs:
   ```bash
   docker-compose -f docker-compose-tor.yml logs -f
   ```

## Configuration

### Fast Sync Configuration (`docker-compose-sync.yml`)
Optimized for initial blockchain download:
- Direct clearnet connections (no Tor)
- Increased database cache (`-dbcache=4096`)
- More connections (`-maxconnections=200`)
- Higher upload target (`-maxuploadtarget=10000`)
- Validation optimizations

### Tor Configuration (`docker-compose-tor.yml`)
Optimized for privacy after sync:
- Routes all traffic through Tor (`-proxy=tor:9050`)
- Listens for connections (`-listen=1`)
- Binds to all interfaces (`-bind=0.0.0.0`)
- Only connects to onion addresses (`-onlynet=onion`)
- Includes known onion nodes for faster connection

## Accessing the Bitcoin node

- The Bitcoin RPC/P2P port is mapped to port 8333 on your host
- The Tor SOCKS5 proxy is available on port 9050

## Data Storage

The Bitcoin blockchain data is stored in `D:/bitcoind` on your host system.

## Network

Both containers operate on an isolated Docker bridge network called `bitcoin-net`.

## Security Considerations

- This setup prioritizes privacy by routing all Bitcoin node traffic through Tor
- No clearnet connections are allowed due to the `-onlynet=onion` setting
- The initial blockchain sync will be slower through Tor but provides better anonymity

## Maintenance

- Keep both Docker images updated regularly for security patches
- Monitor disk space usage as the blockchain grows over time