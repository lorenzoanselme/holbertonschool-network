# Networking Basics #1

## Learning Objectives

### What is localhost / 127.0.0.1
`localhost` is the hostname that refers to the current machine. It maps to the IP address `127.0.0.1` (loopback address), meaning the machine talks to itself. Useful for testing network services locally.

### What is 0.0.0.0
`0.0.0.0` is a non-routable meta-address. In the context of a server, it means **all IPv4 addresses on the local machine**. When a server listens on `0.0.0.0`, it accepts connections on every available network interface.

### What is /etc/hosts
`/etc/hosts` is a plain text file that maps hostnames to IP addresses **before** DNS is consulted. It allows you to override DNS resolution locally. Example:
```
127.0.0.1   localhost
8.8.8.8     facebook.com
```

### How to display active network interfaces
Use the `ifconfig` command (or `ip addr` on modern systems) to display all active network interfaces and their assigned IP addresses.

## Requirements

- Ubuntu 22.04
- All scripts are executable
- Scripts pass Shellcheck 0.7.0

## Files

| File | Description |
|------|-------------|
| `0-change_your_home_IP` | Changes localhost to 127.0.0.2 and facebook.com to 8.8.8.8 in /etc/hosts |
| `1-show_attached_IPs` | Displays all active IPv4 addresses on the machine |
| `2-port_listening_on_localhost` | Listens on port 98 on localhost |
