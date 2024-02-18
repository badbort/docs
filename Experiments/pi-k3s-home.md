# Introduction

Trying k3s with a few raspberry pis


# Setup

## Server

Running server on windows using docker desktop resulted in the k3s client trying to use custom routing to the internal container ips.

This was fixed by adding these `--node-external-ip=<SERVER_EXTERNAL_IP> --flannel-backend=wireguard-native --flannel-external-ip` to the server's command line.

```
docker run -d --privileged --name k3s-server --hostname k3s-server -p 6443:6443 rancher/k3s server --node-external-ip=<windows ip> --flannel-backend=wireguard-native --flannel-external-ip
```

Once installed, get the node token from the server container

```
cat /var/lib/rancher/k3s/server/node-token
```

## Client

```
curl -sfL https://get.k3s.io | K3S_URL=https://<windows ip>:6443 K3S_TOKEN="<node token>" sh -
```

## Tools

