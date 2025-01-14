# OpenVPN and Deluge with WebUI

![Build/Push (master)](https://github.com/ebrianne/docker-deluge-openvpn/workflows/Build/Push%20(master)/badge.svg?branch=master)
[![Docker Pulls](https://img.shields.io/docker/pulls/ebrianne/docker-deluge-openvpn.svg)](https://hub.docker.com/r/ebrianne/docker-deluge-openvpn/)

## Acknowledgments

This project is based heavily on the fork of [docker-transmission-openvpn](https://github.com/haugene/docker-transmission-openvpn). All VPN configurations are now moved to a [separate repository](https://github.com/haugene/vpn-configs-contrib).

## Quick Start

This container contains OpenVPN and Deluge with a configuration
where Deluge is running only when OpenVPN has an active tunnel.
It bundles configuration files for many popular VPN providers to make the setup easier.

```
$ docker run --cap-add=NET_ADMIN -d \
             --sysctl=net.ipv6.conf.all.disable_ipv6=1 \
              -v /your/storage/path/to/downloads/:/download \
              -v /your/storage/path/to/config/:/config \
              -e OPENVPN_PROVIDER=PIA \
              -e OPENVPN_CONFIG=France \
              -e OPENVPN_USERNAME=user \
              -e OPENVPN_PASSWORD=pass \
              -e LOCAL_NETWORK=192.168.0.0/16 \
              -p 8112:8112 \
              ebrianne/docker-deluge-openvpn
```

## Docker Compose
```
version: '3.2'
services:
    deluge-openvpn:
        volumes:
            - '/your/storage/path/to/downloads/:/download'
            - '/your/storage/path/to/config/:/config'
        environment:
            - OPENVPN_PROVIDER=PIA
            - OPENVPN_CONFIG=France
            - OPENVPN_USERNAME=user
            - OPENVPN_PASSWORD=pass
            - LOCAL_NETWORK=192.168.0.0/16
        cap_add:
            - NET_ADMIN
        sysctls:
            - net.ipv6.conf.all.disable_ipv6=1
        ports:
            - '8112:8112'
        image: ebrianne/docker-deluge-openvpn
```
## Documentation

The documentation for this image is hosted on GitHub pages:

https://ebrianne.github.io/docker-deluge-openvpn/

## Access the WEB UI
Access http://HOSTIP:PORT from a browser on the same network. Default password is `deluge`.

## Local Client Access
If you want to access Deluge from a Local client other than the WEB UI, like [Trieme for Android App](https://f-droid.org/packages/org.deluge.trireme/):
Edit the file `/your/storage/path/to/config/auth` to add a new line `username:password:10`, save changes and restart container.

| Credential | Default Value |
| ---------- | ------------- |
| `Host`     | HOST IP       |
| `Port`     | 58846         |
| `Username` | username      |
| `Password` | password      |
