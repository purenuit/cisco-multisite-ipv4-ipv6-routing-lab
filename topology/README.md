# Exact Packet Tracer topology

The full, original lab contains **Marseille (AS200), Lyon (AS100), Paris (AS300)** with eight routers R0–R7, switches, PCs and three inter-AS serial links.

- R1 ↔ R3: `13.0.0.0/30`
- R2 ↔ R4: `24.0.0.0/30`
- R3 ↔ R4: `34.0.0.0/30`
- Marseille interior: R0 ↔ R1, R0 ↔ R2
- Lyon interior: R3 ↔ R5
- Paris interior: R4 ↔ R6 and R4 ↔ R7

## Original network diagram

![Original Multi-Site Network Architecture](plan.png)

See [architecture](../docs/architecture.md) for interface addressing and routing protocols. The original Packet Tracer `.pkt` source project can be added separately.
