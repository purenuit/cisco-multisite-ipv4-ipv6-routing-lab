# Network architecture notes

This page documents the scope of my three-site Cisco routing lab, based on the [original project post](https://www.linkedin.com/posts/william-vainqueur_cisco-networking-routing-activity-7406619213819330560-lNrX).

## Documented technologies

- Sites: **Marseille, Lyon, Paris**
- IPv4 addressing with **FLSM/VLSM** and **IPv6**
- Static routes and routing protocols: **OSPFv2, OSPFv3, RIPv2, RIPng, BGP**
- CLI-based router configuration and troubleshooting

## Conceptual architecture

The network combines interior routing protocols and inter-AS BGP routing. The diagram on the main README represents the project's scope, **not** exact links, routers, area IDs, or AS assignments.

## Missing implementation evidence

| Detail | Status |
|---|---|
| Device inventory and interface connections | Pending original .pkt |
| VLANs and IP addressing plan | Pending original .pkt |
| OSPF processes, areas, adjacencies | Pending configuration |
| RIP networks and redistribution, if any | Pending configuration |
| BGP ASNs, neighbors, advertised prefixes | Pending configuration |
| End-to-end testing results | Pending screenshots / outputs |

Do not treat generic examples as observed configurations from this lab. Once the original project files are shared, document actual devices, protocols, addressing and routing behavior.
