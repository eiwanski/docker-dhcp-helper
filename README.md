# docker-dhcp-helper

A simple, straightforward containerized DHCP relay agent to provide forwarding services to upstream DHCP servers.

## About dhcp-helper

Dockerhub images can be found here: [https://hub.docker.com/r/eiwanski/dhcp-helper](https://hub.docker.com/r/eiwanski/dhcp-helper)


Dockerfile is based on alpine with the dhcp-helper package installed.
The dhcp-helper binary is from the work of [Simon Kelley](https://thekelleys.org.uk/), binaries available at: [https://thekelleys.org.uk/dhcp-helper/](https://thekelleys.org.uk/dhcp-helper/)


Common use cases include:
*   Running a Pi-hole in Docker with DHCP enabled
*   Running a DHCP relay agent in a containerized environment
*   Providing DHCP services to multiple subnets from a single host


## Usage

*   To use multiple relays, specify -s multiple times
*   To use multiple listening interfaces, specify -i multiple times


``` none
dhcp-helper version 1.2-r3, Copyright (C) 2004-2012 Simon Kelley

Usage: dhcp-helper [OPTIONS]
Options are:
-s <server>      Forward DHCP requests to <server>
-b <interface>   Forward DHCP requests as broadcasts via <interface>
-i <interface>   Listen for DHCP requests on <interface>
-e <interface>   Do not listen for DHCP requests on <interface>
-u <user>        Change to user <user> (defaults to nobody)
-r <file>        Write daemon PID to this file (default /var/run/dhcp-helper.pid)
-p               Use alternative ports (1067/1068)
-d               Debug mode
-n               Do not demonize
-v               Give version and copyright info and then exit
```

## docker-compose example

``` yaml
services:
  dhcp-helper:
    name: dhcp-helper
    container_name: dhcp-helper
    hostname: dhcp-helper
    image: eiwanski/dhcp-helper
    network_mode: host
    restart: unless-stopped
    stop_grace_period: 1s
    cap_add:
      - NET_ADMIN
    command: -s 192.168.1.100 -s 192.168.2.100 -i eth0
```
