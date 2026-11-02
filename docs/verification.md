# Verification evidence and follow-up checks

## Observed results in the supplied screenshots

| Source | Destination | Protocol | Observed |
| --- | --- | --- | --- |
| PC2 | 192.168.64.2 | IPv4 | 1 sent, 1 received |
| PC2 | 10.10.0.2 | IPv4 | 1 sent, 1 received |
| PC2 | 10.10.0.130 | IPv4 | 1 sent, 1 received |
| PC2 | 172.16.0.2 | IPv4 | 1 sent, 1 received |
| PC2 | 172.16.4.2 | IPv4 | 1 sent, 1 received |
| PC2 | 172.16.6.2 | IPv4 | 1 sent, 1 received |
| PC61 | 2001:DB8:C:1::2 | IPv6 | 4 sent, 4 received |
| PC61 | 2001:DB8:C:2::2 | IPv6 | 4 sent, 4 received |
| PC61 | 172.16.4.2 | IPv4 | 4 sent, 4 received |
| PC61 | 172.16.6.2 | IPv4 | 4 sent, 4 received |

R4 screenshot: `show ip ospf neighbor` displays router IDs **6.6.6.6** and **7.7.7.7** in **FULL** state; `show ip ospf database` displays OSPF link-state information.

All listed pings succeeded in the captured runs. Results cannot establish sustained reliability, all-to-all IPv6 routing, BGP convergence, failover behavior or end-to-end security.

## Reproducible CLI checklist

```ios
show ip interface brief
show ipv6 interface brief
show ip route
show ipv6 route
show ip protocols
show ipv6 protocols
show ip ospf neighbor
show ipv6 ospf neighbor
show ip ospf database
show ip bgp summary
show ip bgp
```

On PCs, use Packet Tracer's `ping <address>` and `tracert <address>` syntax.

### Additional tests to run
- Verify eBGP **Established** state on all three inter-AS links.
- Confirm that BGP-advertised IPv4 prefixes are truly installed and received.
- From each LAN, test IPv4 and IPv6 reachability to **every other site** (not just local Paris IPv6).
- Inspect static IPv6 inter-AS routing in both directions.
- Test link failure, reconvergence and alternate-route behavior before describing the lab as fault-tolerant.
- Verify `show running-config` rather than relying solely on historical typed commands.

**MP-BGP IPv6 is not used** in the supplied Packet Tracer topology; this limitation is intentionally documented.
