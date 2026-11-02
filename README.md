# Cisco Multi-Site Routing Lab — IPv4 / IPv6

**Marseille (AS 200) · Lyon (AS 100) · Paris (AS 300)**  
Cisco Packet Tracer | 8 Cisco 2911 routers | RIPv2 / RIPng | OSPFv2 / OSPFv3 | eBGP | IPv4 / IPv6

A hands-on, CLI-configured, dual-stack routing lab interconnecting three autonomous systems. This repository now documents the **actual topology and device commands** supplied with the project, rather than a conceptual illustration.

> **Important limitation:** Inter-AS BGP is implemented for **IPv4**. The Packet Tracer router image used in this lab does not support the required MP-BGP IPv6 implementation; the IPv6 inter-AS paths were therefore handled with **static routes**. This is not IPv6 BGP.

## Physical and logical topology

```text
                    MARSEILLE — AS 200
PC0 — S0 — R0 — R1 =====[13.0.0.0/30]===== R3 — R5 — SS1 — PC51
       |     |                                  |        \— SS2 — PC52
PC2 — S2 — R2 =====[24.0.0.0/30]===== R4     LYON — AS 100
                                            ||
                       R3 =====[34.0.0.0/30]===== R4
                                                  | \
                                                  R6  R7
                                                /   \   \
                                             PC61  PC62 PC7
                                                   PARIS — AS 300
```

Above is a **simplified logical map**; refer to the original Packet Tracer file and topology screenshot for the exact connections. Inter-AS links: **R1–R3 (13.0.0.0/30)**, **R2–R4 (24.0.0.0/30)** and **R3–R4 (34.0.0.0/30)**.

| Site | AS | Routers | IPv4 site prefix | IPv6 site prefix | Interior routing |
| --- | --- | --- | --- | --- | --- |
| Marseille | 200 | R0, R1, R2 | 192.168.0.0/16 (split into /18 LANs and /30 links) | 2001:DB8:A::/48 | RIPv2 + RIPng |
| Lyon | 100 | R3, R5 | 10.10.0.0/16 | 2001:DB8:B::/48 | Static routing |
| Paris | 300 | R4, R6, R7 | 172.16.0.0/16 | 2001:DB8:C::/48 | OSPFv2 + OSPFv3 |

### Protocol design

- **Marseille:** RIPv2 for internal IPv4 routes, RIPng for internal IPv6 routes; R1 and R2 are eBGP edges.
- **Lyon:** R3 peers via eBGP with R1 and R4; R5 uses static/default routes for its LANs.
- **Paris:** OSPFv2 (process 10) and OSPFv3 (process 11) use backbone **area 0** and access **areas 1 and 2**. R4 is the eBGP edge; R6 and R7 provide site LAN routing.
- **Routing controls:** configured OSPF costs, router IDs, neighbor timers, interface priorities, passive interfaces, reference bandwidth, route redistribution/default-route experiments and OSPFv2 MD5 authentication.
- **IPv6 inter-AS:** documented static routes, rather than unsupported MP-BGP IPv6.

### Verification evidence

Original screenshots show successful IPv4 probes from PC2 to **192.168.64.2**, **10.10.0.2**, **10.10.0.130**, **172.16.0.2**, **172.16.4.2**, and **172.16.6.2** (1/1 replies each). PC61 shows successful IPv6 probes to **2001:DB8:C:1::2** and **2001:DB8:C:2::2** (4/4 each), and IPv4 probes to **172.16.4.2** and **172.16.6.2** (4/4 each). A router R4 screenshot also displays OSPF neighbors **6.6.6.6** and **7.7.7.7** in FULL state.

These are **individual sampled checks**, not proof of comprehensive reachability, convergence, failover, or throughput.

## Repository guide

| Location | Contents |
| --- | --- |
| [docs/architecture.md](docs/architecture.md) | Detailed site/address/routing design |
| [docs/verification.md](docs/verification.md) | Actual observed results and commands for further checks |
| [configs/](configs/) | Sanitized supplied router command scripts R0–R7 |
| [topology/](topology/) | Original Cisco Packet Tracer project and diagrams (provided in downloadable project bundle) |
| [screenshots/](screenshots/) | Captured routing tests and router CLI evidence (provided in downloadable project bundle) |

**Publication note:** The original binary `.pkt` project and screenshots still need to be uploaded to GitHub separately; the README does not claim they are already hosted here. Router command transcripts are lab notes, **not automatically verified complete running configurations**. Verify current `show running-config` before production-like reuse.

## Technical caveats and lessons learned

- eBGP covers IPv4 in this lab; IPv6 connectivity across AS boundaries requires static routes here.
- Some written commands contain typos, inconsistent network statements or interface-specific values. These are preserved or flagged in the original study material, not assumed to be correct.
- Default routes pointing to a Loopback0 interface, especially `0.0.0.0/0` or `::/0`, are lab experiments and are **not a substitute for a usable upstream next hop**.
- OSPF MD5 shared secrets are redacted in any public configuration.
- Confirm adjacencies, actual installed prefixes, and two-way tests after opening the `.pkt` file.

## Author

**William Vainqueur** · CCNA 200-301 · ITIL v5 · Terraform Associate  
[Original LinkedIn post](https://www.linkedin.com/posts/william-vainqueur_cisco-networking-routing-activity-7406619213819330560-lNrX)
