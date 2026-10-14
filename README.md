<h1 align="center">Connecting Two LANs Using Static Routing</h1>

<p align="center">
  A Cisco Packet Tracer lab that connects two separate LANs through two routers using static routes.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white" alt="Cisco Packet Tracer"/>
  <img src="https://img.shields.io/badge/Routing-Static-blue?style=for-the-badge" alt="Static Routing"/>
  <img src="https://img.shields.io/badge/Level-CCNA-green?style=for-the-badge" alt="CCNA"/>
</p>

---

## Table of Contents

- [Overview](#overview)
- [Network Topology](#network-topology)
- [IP Addressing Table](#ip-addressing-table)
- [Configuration](#configuration)
- [Verification and Testing](#verification-and-testing)
- [Screenshots](#screenshots)
- [Repository Structure](#repository-structure)
- [How to Run](#how-to-run)
- [Skills Demonstrated](#skills-demonstrated)
- [Author](#author)

---

## Overview

This project simulates a small enterprise network in which two LANs, each behind its own router, communicate with each other. Since the routers are only directly aware of their connected networks, **static routes** are configured on both routers so each one can reach the remote LAN.

**Objectives**

- Design and implement a two-LAN topology.
- Assign IPv4 addresses, subnet masks, and default gateways.
- Configure static routes on both routers.
- Verify end-to-end connectivity and troubleshoot using `ping` and routing tables.

---

## Network Topology

| Component | Quantity | Model        |
|-----------|----------|--------------|
| Routers   | 2        | Cisco 2911   |
| Switches  | 2        | Cisco 2960-24TT |
| End Devices | 4      | Laptop-PT    |

```
 [Laptop0]--+                                        +--[Laptop2]
            |                                        |
         [Switch0]---[HQ]===============[BRANCH]---[Switch1]
            |      Gig0/0  Gig0/1   Gig0/0  Gig0/1    |
 [Laptop1]--+                                        +--[Laptop3]

     LAN 1 (192.168.1.0/24)   2.2.2.0/30   LAN 2 (192.168.2.0/24)
```

---

## IP Addressing Table

| Device   | Interface | IP Address    | Subnet Mask     | Default Gateway |
|----------|-----------|---------------|-----------------|-----------------|
| HQ       | Gig0/0    | 192.168.1.1   | 255.255.255.0   | N/A             |
| HQ       | Gig0/1    | 2.2.2.1       | 255.255.255.252 | N/A             |
| BRANCH   | Gig0/1    | 192.168.2.1   | 255.255.255.0   | N/A             |
| BRANCH   | Gig0/0    | 2.2.2.2       | 255.255.255.252 | N/A             |
| Laptop0  | Fa0       | 192.168.1.2   | 255.255.255.0   | 192.168.1.1     |
| Laptop1  | Fa0       | 192.168.1.3   | 255.255.255.0   | 192.168.1.1     |
| Laptop2  | Fa0       | 192.168.2.2   | 255.255.255.0   | 192.168.2.1     |
| Laptop3  | Fa0       | 192.168.2.3   | 255.255.255.0   | 192.168.2.1     |

---

## Configuration

### Interface Configuration

**HQ**
```
HQ(config)# interface gigabitEthernet 0/0
HQ(config-if)# ip address 192.168.1.1 255.255.255.0
HQ(config-if)# no shutdown
HQ(config-if)# exit
HQ(config)# interface gigabitEthernet 0/1
HQ(config-if)# ip address 2.2.2.1 255.255.255.252
HQ(config-if)# no shutdown
```

**BRANCH**
```
BRANCH(config)# interface gigabitEthernet 0/1
BRANCH(config-if)# ip address 192.168.2.1 255.255.255.0
BRANCH(config-if)# no shutdown
BRANCH(config-if)# exit
BRANCH(config)# interface gigabitEthernet 0/0
BRANCH(config-if)# ip address 2.2.2.2 255.255.255.252
BRANCH(config-if)# no shutdown
```

### Static Routes

**HQ** (to reach LAN 2 via BRANCH)
```
HQ(config)# ip route 192.168.2.0 255.255.255.0 2.2.2.2
```

**BRANCH** (to reach LAN 1 via HQ)
```
BRANCH(config)# ip route 192.168.1.0 255.255.255.0 2.2.2.1
```

### Steps Performed

1. Assigned IP addresses to laptops and router interfaces.
2. Enabled router interfaces with `no shutdown`.
3. Configured static routes on both routers.
4. Verified connectivity using `ping`.
5. Inspected routing tables using `show ip route`.
---

## Verification and Testing

| Command | Purpose |
|---------|---------|
| `show ip route` | Confirm static routes (marked with `S`) are installed |
| `show ip interface brief` | Verify interface status and IP addresses |
| `ping <destination-ip>` | Test end-to-end connectivity between LANs |
| `tracert <destination-ip>` | Trace the path between the two LANs |

**Result:** PCs in LAN 1 successfully communicate with PCs in LAN 2 and vice versa.

---

## Screenshots

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/68d1abfb-7aa0-40f5-8552-9a184b414a4b" alt="Full network topology" width="100%"/>
      <br/><sub><b>Network Topology</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/6fb8b254-5dd3-4b94-8f6f-953a49c9ca64" alt="Ping test from PC0 to PC2" width="100%"/>
      <br/><sub><b>Ping Test: PC0 to PC2</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/c516861e-c0d7-4f3b-b434-dba930b9b27d" alt="Ping test from PC3 to PC1" width="100%"/>
      <br/><sub><b>Ping Test: PC3 to PC1</b></sub>
    </td>
  </tr>
</table>

---

## Repository Structure

```
Connecting-Two-LANs-Using-Static-Routing/
├── Two-LAN-Static-Routing.pkt    # Packet Tracer project file
├── screenshots/                  # Topology and test results
└── README.md
```

---

## How to Run

1. Install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer).
2. Download or clone this repository.
3. Open `Two-LAN-Static-Routing.pkt` in Packet Tracer.
4. Open the CLI on each router and run `show ip route` to view the configuration.
5. Use the PC Command Prompt to run `ping` tests between the LANs.

---

## Skills Demonstrated

- IPv4 Addressing and Subnetting
- Default Gateway Configuration
- Static Routing
- Cisco IOS CLI
- ICMP Ping and Network Troubleshooting
- Network Documentation

---

## Author

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/mohamedmousa2003">
        <img src="https://github.com/mohamedmousa2003.png" width="90" style="border-radius:50%" alt="Mohamed Moussa"/><br/>
        <sub><b>Mohamed Moussa</b></sub>
      </a>
    </td>
  </tr>
</table>
