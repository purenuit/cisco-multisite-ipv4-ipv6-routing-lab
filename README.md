# Cisco Multi-Site Routing Lab | IPv4 & IPv6

**Three sites · Dynamic routing · Dual-stack networks · Cisco CLI**

A hands-on networking project exploring routing across a **three-site network** connecting **Marseille, Lyon, and Paris**. I built and configured the lab in Cisco Packet Tracer to strengthen my understanding of IPv4, IPv6, multi-protocol routing, and troubleshooting.

> **Documentation status:** Based on my [original LinkedIn project](https://www.linkedin.com/posts/william-vainqueur_cisco-networking-routing-activity-7406619213819330560-lNrX). The original Packet Tracer file, complete device configurations, IP plan, and screenshots have not yet been added. The conceptual diagram is **not** an exact copy of the original topology.

## Technologies

| Area | Technology |
| --- | --- |
| Locations | Marseille, Lyon, Paris |
| IP addressing | IPv4 (FLSM / VLSM), IPv6 |
| Routing | Static routing, OSPFv2, OSPFv3, RIPv2, RIPng, BGP |
| Configuration | Cisco IOS command-line interface |
| Lab environment | Cisco Packet Tracer |

## Motivation and objectives

I originally explored networking to understand the fundamentals of cybersecurity. The deeper I studied, the more I became passionate about networking itself. This project brought those fundamentals together in a practical multi-site routing scenario.

- Design IPv4 subnet plans using **FLSM and VLSM**.
- Configure and troubleshoot **IPv4/IPv6 dual-stack** connectivity.
- Explore **OSPFv2/OSPFv3**, **RIPv2/RIPng**, and **BGP**.
- Understand the differences between interior and inter-AS routing.
- Validate the network using Cisco IOS show commands, ping, and traceroute.

## Architecture overview

The following is a **conceptual illustration**, not the exact router/link topology:

~~~mermaid
flowchart TB
    C["Inter-site routing<br/>BGP between autonomous systems"]
    M["Marseille<br/>IPv4 + IPv6"]
    L["Lyon<br/>IPv4 + IPv6"]
    P["Paris<br/>IPv4 + IPv6"]
    M --- C
    L --- C
    P --- C
~~~

| Protocol | Role |
| --- | --- |
| OSPFv2 | Interior IPv4 routing |
| OSPFv3 | Interior IPv6 routing |
| RIPv2 | Distance-vector IPv4 routing |
| RIPng | Distance-vector IPv6 routing |
| BGP | Route exchange between autonomous systems |
| Static routes | Explicitly specified paths |

*The exact routing domains and BGP AS numbers will be documented after the original lab files are available.*

## What I learned

This project helped me connect concepts that are often studied independently: subnet design, dynamic routing, IPv6 behavior, route propagation, and end-to-end network testing. It also reinforced the importance of verifying operational state rather than assuming successful configuration means the network works.

## Repository contents

~~~text
.
├── README.md
├── docs/
│   ├── architecture.md
│   └── verification.md
├── configs/
│   └── README.md
├── topology/
│   └── README.md
└── screenshots/
    └── README.md
~~~

Explore the [architecture notes](docs/architecture.md) and [routing verification checklist](docs/verification.md).

## Reproducing the lab

1. Obtain the original Cisco Packet Tracer file (not yet published).
2. Review interfaces, topology, addressing, and routing domains.
3. Check OSPF and BGP neighbors and learned routes as appropriate.
4. Verify IPv4 and IPv6 reachability between sites.
5. Record results and troubleshooting evidence in the repository.

**This repository currently documents the project; it does not yet include a fully reproducible topology.**

## Planned additions

- [ ] Original Cisco Packet Tracer .pkt file
- [ ] Actual topology diagram and interface labels
- [ ] IPv4/IPv6 addressing plan
- [ ] Sanitized router running configurations
- [ ] CLI verification outputs and screenshots

## Author

**William Vainqueur** · CCNA 200-301 · ITIL v5

[Original LinkedIn project post](https://www.linkedin.com/posts/william-vainqueur_cisco-networking-routing-activity-7406619213819330560-lNrX)

*Built to learn, experiment, troubleshoot, and develop my network engineering skills.*
