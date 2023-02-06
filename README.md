# Requirements:
 - Linux x86_64 or Arch64 architecture
 - 4GB RAM
 - 1TB Storage Space (SSD or M.2 recommended)

This guide assumes you are running Fedora Linux x86_64 but can be followed with any other distribution and the only difference is 
when installing dependencies needed. The following packages need to be available on the server before procedding.
 * git
 * docker
 * docker-compose
 * ufw (optional)

## Installing Dependencies
These are the required packages that need to be available in order to run btcpayserver. TMUX is an optional dependency and only being
used during this demo, so you can skip that.

`dnf install docker docker-compose ufw git tmux certbot nginx socat tor`

## Setup firewall
Here we perform some basic firewall hardening by only allowing ports that we need in this case ssh and http ports.

```
ufw default deny incoming
ufw default allow outgoing
for i in ssh http https; do ufw allow $i;done
ufw enable
```

## Setup nginx reverse proxy for SSL termination (i.e manage your own certs)
See official guide at https://docs.btcpayserver.org/FAQ/Deployment/#can-i-use-an-existing-nginx-server-as-a-reverse-proxy-with-ssl-termination

## Clone btcpayserver repo
`git clone https://github.com/btcpayserver/btcpayserver-docker && cd btcpayserver-docker`

## Export environment variables for btcpayserver setup

```
export BTCPAY_HOST="btc.bitcoinwhitepaper.link"
export NBITCOIN_NETWORK="mainnet"
export BTCPAYGEN_CRYPTO1="btc"
export BTCPAYGEN_EXCLUDE_FRAGMENTS="$BTCPAYGEN_EXCLUDE_FRAGMENTS;nginx-https"
export REVERSEPROXY_HTTP_PORT=10080
export REVERSEPROXY_HTTPS_PORT=443
```

## Run install script
`. btcpay-setup.sh -i`

# FAQ

## Using external node with btcpayserver 
https://docs.btcpayserver.org/FAQ/Deployment/#how-to-deploy-btcpay-server-alongside-existing-bitcoin-node

## Official guide
https://docs.btcpayserver.org/Docker/