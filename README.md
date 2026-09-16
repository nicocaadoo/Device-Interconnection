# Device Interconnection — ICPC Network Design

**Final Project**
Tecnológico de Monterrey

## Overview

This project designs, subnets, costs, and simulates (in Cisco Packet Tracer) the campus network infrastructure required to host the **ICPC (International Collegiate Programming Contest)** at Tecnológico de Monterrey.

## Introduction

- **Event:** ICPC — International Collegiate Programming Contest
- **Venue:** Tecnológico de Monterrey
- **Network requirements:**
  - Avoid overload
  - Access to contest platforms and BOCA (the online contest administration system)
  - Reliable communication between organizers

## Problem Statement

The network needed to:
- Integrate into the existing campus infrastructure
- Avoid IP address conflicts
- Guarantee stable connectivity
- Stay up under a large number of simultaneous users

## Objectives

1. **Analyze network requirements**
2. **Design the architecture**
3. **Implement and validate connectivity**

## Capacity Requirements

Minimum capacity of **204 people**:
- 6 teams (30 people each)
- 10 judges
- 2 administrators
- 12 coaches

**Infrastructure components:** switches, routers, an access point, and PCs.

## Network Design

- A hand-sketched layout maps cabling, switches, and routers across the venue (see the Salón de Congresos layout from the earlier venue-selection proposal).
- Implemented and simulated in **Cisco Packet Tracer**:
  - A central router (`Router-Central`) connects to switches `SW-Central-1`, `SW-Central-2`, and `SW-Central-3`.
  - Six team segments (Equipo 1–6), each with its own access switch, color-coded (red, orange, yellow, green, cyan, purple) in the topology.
  - Dedicated PCs for judges (`PC-Jueces`) and administrators (`PC-Admins`).
  - An access point (`AP-Coaches`) for the coaches segment.
- **Internet egress design:** a DNS server (`Server-PT`, 8.8.8.8) and routers (`RSocioFormador`, `RProfesor`) provide access to external services (`www.facebook.com`, `mitec.itesm.mx`, `www.cisco.com`, `www.tec.mx`) and campus-assigned IPs via DHCP. Each team connects to a specific FastEthernet port on the switch based on team number (e.g. Team 3 → FE0/3, Team 10 → FE0/10).

## Subnetting

| Segment | Hosts Required | Prefix | Subnet Mask | Assigned Block | First Valid IP | Last Valid IP |
|---|---|---|---|---|---|---|
| Team 1 | 39/64 | /26 | 255.255.255.192 | 172.20.24.0 – 172.20.24.63 | 172.20.24.1 | 172.20.24.62 |
| Team 2 | 39/64 | /26 | 255.255.255.192 | 172.20.24.64 – 172.20.24.127 | 172.20.24.65 | 172.20.24.126 |
| Team 3 | 39/64 | /26 | 255.255.255.192 | 172.20.24.128 – 172.20.24.191 | 172.20.24.129 | 172.20.24.190 |
| Team 4 | 39/64 | /26 | 255.255.255.192 | 172.20.24.192 – 172.20.24.255 | 172.20.24.193 | 172.20.24.254 |
| Team 5 | 39/64 | /26 | 255.255.255.192 | 172.20.25.0 – 172.20.25.63 | 172.20.25.1 | 172.20.25.62 |
| Team 6 | 39/64 | /26 | 255.255.255.192 | 172.20.25.64 – 172.20.25.127 | 172.20.25.65 | 172.20.25.126 |
| Coaches | 16/32 | /27 | 255.255.255.224 | 172.20.25.128 – 172.20.25.159 | 172.20.25.129 | 172.20.25.158 |
| Judges | 13/16 | /28 | 255.255.255.240 | 172.20.25.160 – 172.20.25.175 | 172.20.25.161 | 172.20.25.174 |
| Admin | 13/16 | /28 | 255.255.255.240 | 172.20.25.176 – 172.20.25.191 | 172.20.25.177 | 172.20.25.190 |

## Equipment & Costs

*Internet egress hardware costs are not included.*

| Equipment | Model | Qty | Unit Cost (USD) | Total (USD) |
|---|---|---|---|---|
| Router | Cisco ISR-4331 | 1 | $4,316 | $4,316 |
| Distribution switch | Cisco 24TT 2960 | 3 | $1,424 | $4,272 |
| Access switch | Cisco 2960 | 6 | $960 | $5,760 |
| Access point | AP-PT | 1 | $650 | $650 |
| PCs (students) | Standard PC | 180 | $500 | $90,000 |
| PCs (judges) | Standard PC | 10 | $500 | $5,000 |
| PCs (administrators) | Standard PC | 2 | $500 | $1,000 |
| **Total** | | | | **$110,998** |

## Switch Configuration (excerpt)

Sample configuration for `SW-E1` (Team 1's access switch): access ports assigned to VLAN 10 (`switchport access vlan 10`, `switchport mode access`), a trunk port for uplink, disabled/unused VLAN 1 management interface, and password-protected console (`line con 0`) and VTY (`line vty`) access.

## Testing

Connectivity validated with `ping`:
- **Team 1 → Team 4:** 4/4 packets received, 0% loss (avg. round-trip ~2 ms)
- **Team 1 → Internet (8.8.8.8):** 4/4 packets received, 0% loss (avg. round-trip ~5 ms)

## Evaluation & Results

- Achieved a functional network
- Efficient segmentation
- Stable connections to the campus network

## Known Issues

- Connecting the internet egress required rework
- The overall proposal had to be redesigned to get everything connected
- Overload on Central Switch 1

## Conclusion & Future Work

- The network meets the stated requirements and performs efficiently
- The simulation passes all connectivity tests
- **Next step:** build the network physically, beyond the Packet Tracer simulation

## Repository Structure


```
├── PacketTracer/       # Cisco Packet Tracer project file (.pkt)
├── Documentation/           # Documentation and delivery
├── Subnetting/               # Subnet calculations
└── README.md
```
