# 3-Tier Architecture Network (NAT + DHCP + DNS + L2 Security) via GNS3

<img width="1920" height="844" alt="Topology" src="https://github.com/user-attachments/assets/4a782d1c-4d94-4af2-9ec1-edb9c11e2b2e" />


## Overview
Simplified collapsed core 3-tier lab showcasing NAT, DHCP, routing, and L2 security. <br>
<br>
Not production ready, has single points of failure and no redundancy.<br>
<br>
Next steps: add dual devices, FHRP, and link redundancy. <br>
<br>
This GNS3 project simulates a network with:

- NAT overload (PAT) for internet access
- DHCP services for internal subnets
- Layer 2 switching / Layer 3 routing 
- DHCP Snooping and Port Security (Sticky MAC)

## Network Design

### WAN

- Public block: 192.168.1.0/30 

- Edge Router (NAT):
   - g0/0 = ISP (VMware Host)
   - g1/0 = Internal network

### Internal Networks
  Network ---- Gateway ----
- 192.168.10.0/24 ---- 192.168.10.254 ---- 
- 192.168.20.0/24 ----	192.168.20.254 ---- 

### DHCP
- Centralized on Distribution Router
- Excluded Addresses: 192.168.10.254 and 192.168.20.254
  - Reserved to keep static IP addresses on access switches.
- Pools:
   - POOL10: 192.168.10.0/24
   - POOL20: 192.168.20.0/24
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

Note: ##

## Author

Kelvin Gonzalez
