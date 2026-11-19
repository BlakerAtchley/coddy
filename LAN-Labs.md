# LAN Labs

**Coddy Computer Networking path, Step 5** · 8 chapters · 35 lessons · 35 graded challenges · Certificate earned 2026-11-14 ([verify](https://coddy.tech/certifications/tj6FfM-AYJDHQ))

[← Back to portfolio summary](README.md)

> **About the commands below:** Coddy's browser terminal grades each challenge live but does not keep a copy of what was typed. The commands here are reference solutions I wrote afterward for each challenge I passed. This course runs in **`netlab`**, Coddy's command-line network simulator. Its commands map one-to-one onto real-world tasks (addressing, gateways, ARP/MAC tables, routing tables, traceroute), and every entry notes the real-world equivalent where it helps.

## What this course covers

Building, addressing and troubleshooting small networks: hosts, switches and routers, cabling, IPv4 addressing, ARP and switch MAC learning, mask mismatches, default gateways and return paths, routing tables, and a structured troubleshooting method (reproduce, read the reason, inspect, fix one thing, re-test). It ends with a capstone repairing a broken branch office.

### `netlab` command reference

| Command | Purpose | Real-world equivalent |
| --- | --- | --- |
| `netlab init` | Create `netlab.json` | — |
| `netlab add host\|switch\|router NAME` | Add a device | Rack a device |
| `netlab link A B` | Cable two devices | Patch cable |
| `netlab show` / `show ip` | Topology / addressing view | `ip addr`, network diagram |
| `netlab ip HOST ADDR/PFX` | Address a host | `ip addr add` / static IP |
| `netlab ip ROUTER IFACE ADDR/PFX` | Address a router interface | Router interface config |
| `netlab gateway HOST ADDR` | Set default gateway | `ip route add default via` |
| `netlab ping A B` / `trace A B` | Reachability / path | `ping` / `traceroute` |
| `netlab show arp HOST` | Host ARP cache | `ip neigh` / `arp -a` |
| `netlab show mac SWITCH` | Switch MAC table | `show mac address-table` |
| `netlab show routes NODE` | Routing table | `ip route` / `show ip route` |
| `netlab check` | Config lint (WARN/ERROR) | Config audit |

## Contents

1. [Meet the Lab](#1-meet-the-lab)
2. [Addresses on the LAN](#2-addresses-on-the-lan)
3. [Masks Matter](#3-masks-matter)
4. [Routers and Gateways](#4-routers-and-gateways)
5. [Troubleshooting I](#5-troubleshooting-i)
6. [Troubleshooting II](#6-troubleshooting-ii)
7. [Growing the Office](#7-growing-the-office)
8. [Capstone: Fix the Branch](#8-capstone-fix-the-branch)
9. [Troubleshooting method](#troubleshooting-method)

---

## 1. Meet the Lab

### 1.1 Your First Lab · Beginner
**Task:** Create a lab with hosts `pc1` and `pc2`, then show it.
```bash
netlab init
netlab add host pc1
netlab add host pc2
netlab show
```

### 1.2 Cables · Beginner
**Task:** Add switch `sw1`, cable both hosts to it (pc1 first), and show the lab.
```bash
netlab add switch sw1
netlab link pc1 sw1
netlab link pc2 sw1
netlab show
```
Switch ports are assigned in the order you plug in, so pc1 gets port1 and pc2 gets port2.

### 1.3 Wires Are Not Enough · Beginner
**Task:** Show the addressing view, then try to ping between hosts that have no IPs.
```bash
netlab show ip
netlab ping pc1 pc2
```
Result: `unreachable (pc1 has no IP address)`. A cable alone isn't enough; L3 needs addresses.

### 1.4 Recap: Build a Lab · Easy
**Task:** From scratch, build a laptop and a printer on switch `office`.
```bash
netlab init
netlab add host laptop
netlab add host printer
netlab add switch office
netlab link laptop office
netlab link printer office
netlab show
```

---

## 2. Addresses on the LAN

### 2.1 Giving a Host an Address · Beginner
**Task:** Address pc1 as `.10/24` and pc2 as `.20/24` in `192.168.1.0`.
```bash
netlab ip pc1 192.168.1.10/24
netlab ip pc2 192.168.1.20/24
```
The lab echoes back the mask and network it derived, a quick check that both hosts landed in `192.168.1.0/24`.

### 2.2 The First Ping · Beginner
**Task:** Ping in both directions.
```bash
netlab ping pc1 pc2
netlab ping pc2 pc1
```
TTL is 64 in both directions because no router was crossed.

### 2.3 What ARP Learned · Beginner
**Task:** Show pc1's ARP table before and after a ping.
```bash
netlab show arp pc1
netlab ping pc1 pc2
netlab show arp pc1
```
The table starts empty. After the ping it holds `192.168.1.20 → 02:c0:dd:a0:9f:64`.

### 2.4 The Switch Learns Too · Beginner
**Task:** Ping, then show what `sw1` learned.
```bash
netlab ping pc1 pc2
netlab show mac sw1
```
The switch maps each source MAC to the port it arrived on, so it can forward frames to one port instead of flooding all of them.

### 2.5 Recap: First Conversation · Easy
**Task:** Address the laptop and the printer in `10.0.0.0/24`, ping, and show the MAC table.
```bash
netlab ip laptop 10.0.0.11/24
netlab ip printer 10.0.0.12/24
netlab ping laptop printer
netlab show mac office
```

---

## 3. Masks Matter

### 3.1 Wrong Mask, No Reply · Easy
**Task:** pc2 was set to `/25` by mistake. Diagnose it, fix the mask, and re-test.
```bash
netlab ping pc1 pc2          # no reply: pc2 thinks pc1 is off-subnet
netlab ip pc2 192.168.1.200/24
netlab ping pc1 pc2
```
This is a one-way failure: the request arrives, but pc2 (with `/25`) believes `.10` is on another network and has no gateway for the reply.

### 3.2 Same Switch, Other Subnet · Easy
**Task:** pc2 is in `192.168.2.0/24` by mistake. Move it back.
```bash
netlab ping pc1 pc2
netlab ip pc2 192.168.1.20/24
netlab ping pc1 pc2
```
A shared switch (L2) doesn't put hosts in the same subnet (L3).

### 3.3 Let check Do the Looking · Beginner
**Task:** Run the configuration lint on a hastily built lab.
```bash
netlab check
```
It found 3 problems: pc3 has no IP, and pc4 has no cable and no gateway.

### 3.4 Recap: Diagnose the Pair · Easy
**Task:** Host b's gateway is outside its subnet. Fix b's address and test.
```bash
netlab check
netlab ip b 10.0.0.6/24
netlab ping a b
```
b was in `10.0.1.0/24` while its gateway was `10.0.0.1`. Moving b into `10.0.0.0/24` fixes both its subnet and its gateway.

---

## 4. Routers and Gateways

### 4.1 Two Networks · Easy
**Task:** Give router r1 the `.1` in each of its two networks.
```bash
netlab ip r1 eth0 192.168.1.1/24
netlab ip r1 eth1 10.0.0.1/24
netlab show ip
```

### 4.2 The Default Gateway · Easy
**Task:** Set pc1's gateway and watch the failure move.
```bash
netlab ping pc1 pc3                  # unreachable: pc1 has no gateway
netlab gateway pc1 192.168.1.1
netlab ping pc1 pc3                  # no reply: pc3 can't answer back
```

### 4.3 Both Sides Need a Way Back · Easy
**Task:** Give pc3 a gateway, then ping and trace.
```bash
netlab gateway pc3 10.0.0.1
netlab ping pc1 pc3
netlab trace pc1 pc3
```
The reply shows `ttl=63, via r1`: the router decremented the TTL by one. The trace lists `r1` then `pc3`.

### 4.4 Routing Tables · Beginner
**Task:** Compare a host's routing table with a router's.
```bash
netlab show routes pc1
netlab show routes r1
```
The host has its connected subnet plus a `0.0.0.0/0` default route via its gateway. The router has one connected route per interface.

### 4.5 Recap: Connect the Floors · Medium
**Task:** Route between two office floors.
```bash
netlab ip core eth0 172.16.1.1/24
netlab ip core eth1 172.16.2.1/24
netlab gateway a1 172.16.1.1
netlab gateway b1 172.16.2.1
netlab ping a1 b1
```

---

## 5. Troubleshooting I

### 5.1 The Unplugged Host · Easy
**Task:** "The server is down." Find the missing cable and fix it.
```bash
netlab ping pc1 pc3     # unreachable: r1 eth1 not connected
netlab show             # pc3 LINKS = "-"
netlab link r1 pc3
netlab ping pc1 pc3
```

### 5.2 The Missing Gateway · Easy
**Task:** Fix the side that can't send its reply.
```bash
netlab ping pc1 pc3
netlab gateway pc3 10.0.0.1
netlab ping pc1 pc3
```
Fix the machine **named in the error message**, not the one you're typing on.

### 5.3 The Typo in the Mask · Easy
**Task:** Find the odd mask in the addressing view and correct it.
```bash
netlab show ip                      # pc2 is /25 (255.255.255.128)
netlab ping pc1 pc2
netlab ip pc2 192.168.1.130/24
netlab ping pc1 pc2
```
With `/25`, a host at `.130` believes everything below `.128` is a different network.

### 5.4 Recap: Two Faults · Medium
**Task:** Fix an unaddressed host and a gateway that points at an address no device owns.
```bash
netlab check                         # pc2: no IP address
netlab ip pc2 192.168.1.20/24
netlab gateway pc2 192.168.1.1
netlab ping pc3 pc1                  # no device at gateway 10.0.0.9
netlab gateway pc3 10.0.0.1
netlab ping pc3 pc1
```
`check` can't catch a gateway that's in the right subnet but owned by nobody. Only a ping exposes it.

---

## 6. Troubleshooting II

### 6.1 Two Devices, One Address · Easy
**Task:** Resolve an IP conflict.
```bash
netlab ping pc1 pc2
netlab check                         # ERROR 192.168.1.20 used by pc2 and pc4
netlab ip pc4 192.168.1.40/24
netlab ping pc1 pc2
```
Move the newcomer so the established machine keeps its address.

### 6.2 The Network Address Trap · Easy
**Task:** pc2 was given the subnet's network address. Fix it.
```bash
netlab check
netlab ip pc2 192.168.1.20/24
netlab ping pc1 pc2
```
The network address (`.0`) and broadcast address (`.255`) can never be assigned to a host.

### 6.3 Gateway on the Wrong Subnet · Easy
**Task:** pc1's gateway is the router's address on the *other* network. Fix it.
```bash
netlab ping pc1 pc3
netlab check
netlab gateway pc1 192.168.1.1
netlab ping pc1 pc3
```
A gateway must be inside the host's own subnet so the host can reach it without routing.

### 6.4 Recap: The Silent Router · Medium
**Task:** The hosts are fine, but the router is missing an interface address.
```bash
netlab ping pc1 pc3              # r1 has no route to 10.0.0.20
netlab show routes r1            # only 192.168.1.0/24 connected
netlab ip r1 eth1 10.0.0.1/24
netlab ping pc1 pc3
```

---

## 7. Growing the Office

Build a two-department office step by step: one core router, one switch and one subnet per department.

### 7.1 The Plan · Easy
**Task:** Lay down the infrastructure: router `core` and switches `sales` and `eng`.
```bash
netlab init
netlab add router core
netlab add switch sales
netlab add switch eng
netlab link core sales
netlab link core eng
netlab show
```
Cabling Sales first gives it `core eth0`.

### 7.2 Sales Comes Online · Easy
**Task:** Bring Sales up on `10.10.1.0/24`.
```bash
netlab add host sales1
netlab add host sales2
netlab link sales1 sales
netlab link sales2 sales
netlab ip sales1 10.10.1.11/24
netlab ip sales2 10.10.1.12/24
netlab ip core eth0 10.10.1.1/24
netlab ping sales1 sales2
```

### 7.3 Engineering Joins · Easy
**Task:** Bring Engineering up on `10.10.2.0/24` and show that the gateway gap is real.
```bash
netlab add host eng1
netlab link eng1 eng
netlab ip eng1 10.10.2.11/24
netlab ip core eth1 10.10.2.1/24
netlab ping eng1 sales1          # unreachable: no gateway yet
```

### 7.4 Gateways Everywhere · Easy
**Task:** Give every desktop a gateway, then ping and trace across departments.
```bash
netlab gateway sales1 10.10.1.1
netlab gateway sales2 10.10.1.1
netlab gateway eng1 10.10.2.1
netlab ping eng1 sales1
netlab trace eng1 sales2
```

### 7.5 Recap: The Printer · Medium
**Task:** Add a shared Sales printer, reachable from Engineering, and finish with a clean check.
```bash
netlab add host printer
netlab link printer sales
netlab ip printer 10.10.1.50/24
netlab gateway printer 10.10.1.1
netlab ping eng1 printer
netlab check                     # Lab check: no problems found
```

---

## 8. Capstone: Fix the Branch

A contractor left a two-floor branch (`192.168.10.0/24` on s1, `192.168.20.0/24` on s2, router `br`) half-broken.

### 8.1 Survey the Damage · Easy
**Task:** Look before touching anything.
```bash
netlab show
netlab check
```
Findings: h4 has no gateway, h3 and h5 share `192.168.20.11`, and h2 has a `/25` mask on a `/24` floor (two warnings). 4 problems in total.

### 8.2 Fix the Address Clash · Easy
**Task:** Move h5 to a free address and test.
```bash
netlab ip h5 192.168.20.13/24
netlab ping h5 h3
```

### 8.3 Masks and Gateways · Medium
**Task:** Fix h2's mask and the second-floor gateways (h3's pointed at `.9`, which no device owns).
```bash
netlab ip h2 192.168.10.12/24
netlab gateway h3 192.168.20.1
netlab gateway h4 192.168.20.1
netlab check                     # no problems found
```

### 8.4 Recap: Prove It Works · Easy
**Task:** Prove the repair end to end.
```bash
netlab ping h1 h3
netlab ping h2 h4
netlab trace h4 h1
```

---

## Troubleshooting method

1. **Reproduce:** ping the reported path.
2. **Read the reason:** "unreachable" means the sender can't send, and "no reply" means the far side can't answer back.
3. **Inspect:** use `show` (cabling), `show ip` (addressing and masks), `show routes` (paths) and `check` (config lint).
4. **Fix one thing**, then **re-test**, including the reverse direction.

The same loop applies to Azure: check the NIC IP and subnet, then effective routes (UDRs), then NSG rules, then confirm with Connection Troubleshoot / Network Watcher.
