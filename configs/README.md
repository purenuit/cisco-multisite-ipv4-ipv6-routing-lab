# Router configuration transcripts

The project contains CLI command sequences for **R0 through R7**. These reflect the configuration **as documented during the lab**, not guaranteed complete `show running-config` exports.

| Router | Function |
| --- | --- |
| R0 | Marseille internal RIPv2 / RIPng |
| R1 | Marseille eBGP edge to Lyon |
| R2 | Marseille eBGP edge to Paris |
| R3 | Lyon eBGP edge to Marseille and Paris |
| R4 | Paris eBGP edge and OSPF backbone |
| R5 | Lyon LAN and default static routing |
| R6 | Paris area 1 OSPFv2/OSPFv3 |
| R7 | Paris area 2 OSPFv2/OSPFv3 |

**Security:** The provided OSPFv2 MD5 keys are lab secrets, and must not be published. The downloadable bundle replaces them with placeholders. The GitHub-hosted docs do not expose keys.

**Accuracy:** Several source lines require validation (R4's OSPF IPv4 wildcard networks, R5's default next hop, R7's initial mask and some BGP network statements). Compare the command transcripts to the live Packet Tracer running configuration before replaying them blindly.
