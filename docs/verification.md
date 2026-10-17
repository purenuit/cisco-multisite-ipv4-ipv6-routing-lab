# Cisco IOS routing verification checklist

These are **suggested commands**, not claimed test results. Syntax and support depend on IOS/Packet Tracer version.

## Interfaces and addresses

```text
show ip interface brief
show ipv6 interface brief
show running-config
```

- Verify up/up interfaces, correct IPv4/IPv6 addressing and expected active configurations.
- Remove passwords, credentials, keys and public IPs if needed before uploading configurations.

## IPv4 and IPv6 route tables

```text
show ip route
show ipv6 route
```

Check destination prefixes and next hops. The presence of a route does not alone prove connectivity.

## OSPFv2 / OSPFv3

```text
show ip ospf neighbor
show ip ospf interface
show ipv6 ospf neighbor
```

Check expected neighbors and adjacency stability. Older Packet Tracer images may not support every command.

## RIPv2 / RIPng

```text
show ip protocols
show ipv6 protocols
```

Verify configured processes and the presence of appropriate RIP-learned routes in the routing table.

## BGP

```text
show ip bgp summary
show ip bgp
show bgp ipv6 unicast summary
show bgp ipv6 unicast
```

Use the platform-supported command variants and confirm that expected sessions and learned prefixes exist.

## End-to-end connectivity

```text
ping <destination-ipv4>
ping ipv6 <destination-ipv6>
traceroute <destination-ipv4>
traceroute ipv6 <destination-ipv6>
```

Replace placeholders with actual lab addresses. Validate local, inter-subnet and cross-site paths.

## Evidence to collect

- [ ] Interface and IP addressing outputs
- [ ] IPv4 and IPv6 routing tables
- [ ] OSPF neighbor states
- [ ] RIP learned routes
- [ ] BGP session states and advertised/learned prefixes
- [ ] Cross-site ping and traceroute outputs
- [ ] Troubleshooting notes and fixes
