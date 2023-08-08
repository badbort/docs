# Introduction

Messing around with k3s client on a pi. Also running k3s server on WSL2 instance

#### Common

Just dumping things I've had to use

| Intent | Command |
|---|---|
| Get IP | `hostname -I` |
| List Services | `sudo systemctl --all --type=service `


#### k3s: 
Get service logs
```
sudo journalctl -u k3s-agent.service
```

Uninstall
```
/usr/local/bin/k3s-uninstall.sh
```

#### Route PC port to wsl2 port for accessing k3s:
```
netsh interface portproxy add v4tov4 listenport=6443 listenaddress=0.0.0.0 connectport=6443 connectaddress=172.23.58.22
```
See: https://github.com/microsoft/WSL/issues/4150
