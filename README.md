# Enterprise Network (NAT + DHCP + DNS + L2 Security) via GNS3

<img width="1918" height="969" alt="Topology" src="https://github.com/user-attachments/assets/736c9535-3e5d-47e6-af51-525ec1e785b1" />

## Overview

This GNS3 simulates a small enterprise network with:

- NAT overload (PAT) for internet access
- DHCP services for internal subnets
- Layer 2 switching / Layer 3 routing 
- DHCP Snooping and Port Security (Sticky MAC)

## Network Design

### WAN

- Public block: 192.168.1.0/30 (VMware Host's IP address)
- Edge Router (NAT):
   - g0/0 = ISP
   - g1/0 = Internal network

### Internal Networks
  Network ---- Gateway ----Device
- 192.168.10.0/24 ---- 192.168.10.254 ---- L2Switch-1
- 192.168.20.0/24 ----	192.168.20.254 ---- L2Switch-2

### DHCP
- Centralized on Distribution Router
- Excluded Addresses: 192.168.10.254 and 192.168.20.254
  - Reserved to keep static IP addresses on access switches.
- Pools:
   - POOL1: 192.168.1.0/24
   - POOL2: 192.168.2.0/24
- DNS: 8.8.8.8
- Domain: kelvin.lab.local

### NAT
- Inside: g1/0 <br>
- Outside: g0/0 <br>
PAT using access-list: <br>
access-list 1 permit 192.168.10.0 0.0.0.255 <br>
access-list 1 permit 192.168.20.0 0.0.0.255 <br>
- NAT Overload: g0/0

### Layer 2 Security
- DHCP Snooping enabled
  - Trusted ports configured toward router/uplinks
  - Limit rate of 4 DHCP packets per second
- Port Security (Sticky MAC)
   - Enabled on switch ports connected to end-users
   - Violation mode typically set to restrict 

### Features Implemented
- DHCP services
- NAT overload (PAT)
- DNS reachability
- L2 security protections against spoofing and unauthorized rogue device plug-ins

## Author

Kelvin Gonzalez
