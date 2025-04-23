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

1. Clone this repository
2. Make sure you have the directory `D:/bitcoind` created (or modify the volume path in `docker-compose.yml`)
3. Start the services with:
   ```
   docker-compose up -d
   ```
4. Check the logs with:
   ```
   docker-compose logs -f
   ```

## Configuration

The Bitcoin node is configured with the following parameters:
- Routes all traffic through Tor (`-proxy=tor:9050`)
- Listens for connections (`-listen=1`)
- Binds to all interfaces (`-bind=0.0.0.0`)
- Only connects to onion addresses (`-onlynet=onion`)

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