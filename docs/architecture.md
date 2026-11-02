# Architecture and addressing — original three-site lab

This page is grounded in the supplied Packet Tracer diagram and the R0–R7 command transcripts. Prefixes below describe the **intended design**; configuration and live control-plane state must be checked in Packet Tracer.

## Autonomous systems and routing
| AS | City | Routers | IPv4 IGP / routing | IPv6 IGP / routing |
| --- | --- | --- | --- | --- |
| 200 | Marseille | R0, R1, R2 | RIPv2 + eBGP edges | RIPng + static inter-AS routes |
| 100 | Lyon | R3, R5 | Static LAN routes + eBGP | Static routes |
| 300 | Paris | R4, R6, R7 | Multi-area OSPFv2 + eBGP edge | Multi-area OSPFv3 + static inter-AS routes |

## Device-by-device roles
| Router | Role | Connections |
| --- | --- | --- |
| R0 | Marseille internal RIP router | G0/0 192.168.0.1/18; G0/1 192.168.128.1/30 → R1; G0/2 192.168.128.5/30 → R2 |
| R1 | Marseille edge | G0/1 192.168.128.2/30; S0/0/0 13.0.0.1/30 → R3; eBGP AS200 to AS100 |
| R2 | Marseille edge | G0/0 192.168.64.1/18; G0/2 192.168.128.6/30; S0/0/0 24.0.0.1/30 → R4; eBGP AS200 to AS300 |
| R3 | Lyon edge | G0/2 10.10.1.1/30 → R5; S0/0/0 13.0.0.2/30 → R1; S0/0/1 34.0.0.1/30 → R4; eBGP AS100 |
| R4 | Paris edge / OSPF backbone | G0/2 172.16.6.65/30 → R6; G0/1 172.16.6.69/30 → R7; S0/0/0 24.0.0.2/30 → R2; S0/0/1 34.0.0.2/30 → R3; eBGP AS300 |
| R5 | Lyon LAN router | G0/0 10.10.0.1/25; G0/1 10.10.0.129/25; G0/2 10.10.1.2/30 |
| R6 | Paris OSPF ABR | G0/2 172.16.6.66/30 (area 0); G0/0 172.16.0.1/22 and G0/1 172.16.4.1/23 (area 1) |
| R7 | Paris OSPF ABR | G0/1 172.16.6.70/30 (area 0); G0/0 172.16.6.1/26 (area 2) |

## IPv6 addressing
- Marseille: `2001:DB8:A::/48`; RIPng process `VAINQUEUR`.
- Lyon: `2001:DB8:B::/48`; R3–R5 transit `2001:DB8:B:2::/64`.
- Paris: `2001:DB8:C::/48`; R4–R6 `2001:DB8:C:3::/64`; R4–R7 `2001:DB8:C:4::/64`; R6 LANs `2001:DB8:C::/64`, `2001:DB8:C:1::/64`; R7 LAN `2001:DB8:C:2::/64`.
- Inter-AS IPv6 transit: `2001:DB8:13::/64` (R1–R3), `2001:DB8:24::/64` (R2–R4), `2001:DB8:34::/64` (R3–R4).

## Paris OSPF
- IPv4: process **10**, area **0** backbone, area **1** via R6, area **2** via R7.
- IPv6: process **11**, with corresponding area partitioning and explicit router IDs.
- Tuned interface cost, timers and priorities; OSPFv2 message-digest (MD5) authentication configured.
- One supplied screenshot shows R4 with R6 and R7 as **FULL** OSPFv2 neighbors. This does **not** independently validate every OSPFv3 adjacency.

## eBGP peering
| Peer A | Peer B | eBGP |
| --- | --- | --- |
| R1 (AS200, 13.0.0.1) | R3 (AS100, 13.0.0.2) | IPv4 |
| R2 (AS200, 24.0.0.1) | R4 (AS300, 24.0.0.2) | IPv4 |
| R3 (AS100, 34.0.0.1) | R4 (AS300, 34.0.0.2) | IPv4 |

