# Multi-Site Infrastructure Lab — IPv4 / IPv6 Addressing & Routing

**Cisco Packet Tracer | Marseille · Lyon · Paris | Configuration entirely through Cisco IOS CLI**

## 1. Project overview

This lab simulates a multi-site network infrastructure connecting **Marseille, Lyon and Paris**. I configured the routers entirely from the command line, combining IPv4 subnetting (FLSM/VLSM), IPv6 addressing, interior routing protocols and interconnection between distinct autonomous systems.

The goal was to understand how addressing choices, routing protocol boundaries and router configuration affect end-to-end connectivity across several sites.

## 2. The network I designed

**The original Packet Tracer topology is the reference for this project.** No generated or alternative diagram is used. The actual topology diagrams are `plan.png` and `plan2.png`.

The lab is divided into three autonomous systems:

| Site | Autonomous system | Routers | IPv4 site range | IPv6 site range |
| --- | --- | --- | --- | --- |
| Marseille | AS 200 | R0, R1, R2 | 192.168.0.0/16 | 2001:DB8:A::/48 |
| Lyon | AS 100 | R3, R5 | 10.10.0.0/16 | 2001:DB8:B::/48 |
| Paris | AS 300 | R4, R6, R7 | 172.16.0.0/16 | 2001:DB8:C::/48 |

The links between autonomous systems connect **R1–R3**, **R2–R4** and **R3–R4**. The actual Packet Tracer screenshots show individual LAN segments, point-to-point links, switch connections and addressing labels.

## 3. Addressing: building the foundation

I began by assigning **IPv4 and IPv6 addresses** across the site LANs and inter-router links. The network uses different subnet sizes depending on the role of each connection: LAN subnets for hosts and /30 IPv4 networks for point-to-point connections.

For example:
- Marseille hosts use the 192.168.0.0/18 and 192.168.64.0/18 LANs.
- Lyon has two 10.10.0.0/25 and 10.10.0.128/25 host networks.
- Paris uses 172.16.0.0/22, 172.16.4.0/23 and 172.16.6.0/26 LANs.
- The three inter-AS IPv4 links use 13.0.0.0/30, 24.0.0.0/30 and 34.0.0.0/30.

All three sites also have IPv6 prefixes with /64 subnets for individual links and LANs.

## 4. Routing within each site

### Marseille — RIPv2 and RIPng

On **R0, R1 and R2**, I worked with **RIPv2 for IPv4** and **RIPng for IPv6**. This gave me a practical example of distance-vector routing for a limited internal scope, with interface controls and route propagation between the routers.

### Lyon — static routing

On the Lyon site, **R3 and R5** connect the local networks to the other autonomous systems. R5's configuration includes IPv4/IPv6 default routing, while R3 connects the site to the inter-AS routing infrastructure. This demonstrates the role of explicit next-hop routes in a smaller routing domain.

### Paris — multi-area OSPFv2 and OSPFv3

On **R4, R6 and R7**, I configured hierarchical routing with **OSPFv2 (IPv4)** and **OSPFv3 (IPv6)**. The design includes **area 0** for backbone links and separate **areas 1 and 2** for the Paris LAN segments.

I also worked with **OSPF costs, interface priorities, Hello/Dead timers, passive interfaces and OSPFv2 MD5 authentication**, as shown in the router CLI screenshots. These parameters gave me a way to explore routing behavior beyond simply enabling a protocol.

## 5. Interconnecting three autonomous systems

After establishing the site routing design, I configured **eBGP for IPv4 route exchange** between three autonomous systems:

| Routers | Autonomous systems | IPv4 interconnection |
| --- | --- | --- |
| R1 ↔ R3 | AS 200 ↔ AS 100 | 13.0.0.0/30 |
| R2 ↔ R4 | AS 200 ↔ AS 300 | 24.0.0.0/30 |
| R3 ↔ R4 | AS 100 ↔ AS 300 | 34.0.0.0/30 |

The router captures also document the use of **loopback interfaces, default routes and static routing**. In this Packet Tracer lab, inter-AS IPv6 routing was addressed using static routes rather than claiming MP-BGP IPv6 support.

## 6. Testing and validation

After configuration, I checked connectivity using the Packet Tracer PC command prompts and Cisco IOS diagnostic commands.

**IPv4 — PC2:** the captured one-packet tests received replies from all of the following destinations:

| Destination | Result |
| --- | --- |
| 192.168.64.2 | 1/1 replies |
| 10.10.0.2 | 1/1 replies |
| 10.10.0.130 | 1/1 replies |
| 172.16.0.2 | 1/1 replies |
| 172.16.4.2 | 1/1 replies |
| 172.16.6.2 | 1/1 replies |

**IPv6 — PC61:** `2001:DB8:C:1::2` and `2001:DB8:C:2::2` each returned **4/4 replies**. The same PC also reached IPv4 addresses `172.16.4.2` and `172.16.6.2` with **4/4 replies**.

**OSPF adjacency — R4:** `show ip ospf neighbor` shows R6 (`6.6.6.6`) and R7 (`7.7.7.7`) in **FULL** state. `show ip ospf database` was also used to inspect the link-state database.

These screenshots demonstrate the listed successful tests. They do **not**, by themselves, prove every possible cross-site IPv6 route, convergence scenario or redundancy test.

## 7. Router configuration — Cisco IOS commands

The following transcripts are based on the supplied router materials. They document commands used during configuration rather than certified final running configurations. The original CLI screenshots should be viewed alongside them. Shared authentication keys are redacted.

| Router | CLI commands |
| --- | --- |
| Marseille R0 | [R0 commands](configs/R0-cli-excerpt.txt) |
| Marseille R1 | [R1 commands](configs/R1-cli-excerpt.txt) |
| Marseille R2 | [R2 commands](configs/R2-cli-excerpt.txt) |
| Lyon R3 | [R3 commands](configs/R3-cli-excerpt.txt) |
| Lyon R5 | [R5 commands](configs/R5-cli-excerpt.txt) |
| Paris R4 | [R4 commands](configs/R4-cli-excerpt.txt) |
| Paris R6 | [R6 commands](configs/R6-cli-excerpt.txt) |
| Paris R7 | [R7 commands](configs/R7-cli-excerpt.txt) |

**Illustrative commands from the original configuration:**

```ios
! Paris / R6 : OSPFv2 (process 10)
router ospf 10
 router-id 6.6.6.6
 network 172.16.6.64 0.0.0.3 area 0
 network 172.16.0.0 0.0.3.255 area 1
 network 172.16.4.0 0.0.1.255 area 1

! Paris / R6 : OSPFv3 (process 11)
ipv6 router ospf 11
 router-id 6.6.6.6
interface g0/2
 ipv6 ospf 11 area 0

! Inter-AS IPv4 BGP : R3 / AS100
router bgp 100
 neighbor 13.0.0.1 remote-as 200
 neighbor 34.0.0.2 remote-as 300
```

Some source command lines contain apparent errors or inconsistencies, notably R5's IPv4 default next hop and R7's initial LAN subnet mask. They are documented, not silently corrected or claimed as successfully applied.

## 8. What this lab taught me

This project connected several routing topics into one practical exercise: how to plan networks, assign subnets, configure interfaces and routing processes, connect autonomous systems, and then confirm the results from the network devices and PCs.

It also reinforced that a correct-looking configuration is only a starting point. Verifying routes, neighbor relationships and actual reachability is essential.

## Original project materials

- **Topology:** original `plan.png` and `plan2.png` screenshots and `PLAN ADDRESSAGE.pkt` file
- **Router configuration evidence:** CLI screenshots from **R0 through R7**
- **Validation:** `PINGIPV4.png`, `PING OSPFV6V4.png` and OSPF diagnostic captures

> The screenshots and Packet Tracer project are the authoritative source. This text is a narrative of the configuration and results they show; it is not a generated alternative topology.

**Tools:** Cisco Packet Tracer · Cisco IOS CLI · IPv4 / IPv6 · RIPv2 / RIPng · OSPFv2 / OSPFv3 · BGP

[Original LinkedIn project post](https://www.linkedin.com/posts/william-vainqueur_cisco-networking-routing-activity-7406619213819330560-lNrX)
