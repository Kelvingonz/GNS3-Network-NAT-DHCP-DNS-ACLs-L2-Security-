# 3-Tier Architecture Network (NAT + DHCP + DNS + L2 Security)

<img width="1920" height="902" alt="Topology" src="https://github.com/user-attachments/assets/84b13be9-0c30-46ce-a2f4-b9e8129b3784" />
<br>
Simplified collapsed core 3-tier architecture network showcasing NAT, DHCP, DNS, ACLs, switching, routing, and L2 security using GNS3 VM. <br>

## Overview
This lab was built using Cisco Virtual IOS L2 (vIOS L2) switch images and Cisco 7200 IOS (C7200-ADVENTERPRISEK9-M) router images.

This is a non-production environment used for testing networking protocols, configurations, and security features in a controlled setup.
<br>
<br> Faults: Has single points of failure and no redundancy. Ideally dual devices, FHRP, and link redundancy should be added but I will leave that for future project.
<br>

This GNS3 project simulates a network with:

- NAT overload (PAT) for internet access
- Use ACLs to identify which traffic should be translated via PAT 
- DHCP services for internal subnets
- Layer 2 switching / Layer 3 routing 
- DHCP Snooping and Port Security (Sticky MAC)

## Network Design

### WAN

- Public block: 192.168.1.0/24

- Edge Router (NAT):
   - g0/0 = ISP (Network Adapter VMnet0 - Bridged)
   - g1/0 = Point-to-point Link with Distribution Router

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

### NAT + ACLs
- Inside: g1/0 <br>
- Outside: g0/0 <br>

NAT Overload PAT using access-list: <br>
ip access-list standard PAT <br>
permit 192.168.10.0 0.0.0.255 <br>
permit 192.168.20.0 0.0.0.255 <br>

- ip nat inside source list PAT interface GigabitEthernet0/0 overload

### Layer 2 Security
- DHCP Snooping enabled
  - Trusted ports configured toward router/uplinks
  - Limit rate of 4 DHCP packets per second
- Port Security (Sticky MAC)
   - Enabled on switch ports connected to end-users
   - Maximum MAC addresses allowed on each switch: 2
   - Violation mode typically set to restrict 

### Features Implemented
- DHCP services
- NAT overload (PAT)
- DNS reachability
- L2 security protections against spoofing and unauthorized rogue device plug-ins

## Author

Kelvin Gonzalez
