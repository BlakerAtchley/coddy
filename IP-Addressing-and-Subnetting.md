# IP Addressing and Subnetting

**Coddy Computer Networking path, Step 3** · 10 chapters · 46 lessons · 46 graded challenges · Certificate earned 2026-11-08 ([verify](https://coddy.tech/certifications/tj6FfM-LDYK7g))

[← Back to portfolio summary](README.md)

> **About the commands below:** Coddy's browser terminal grades each challenge live but does not keep a copy of what was typed. The commands here are reference solutions I wrote afterward. Most challenges are "work it out, then print the answer", so each entry shows the **working** (the actual subnetting math) and then the command that prints the graded result. Where Bash arithmetic can do the math, I've shown that too.

## What this course covers

IP addressing from the bits up: binary/decimal conversion, IPv4 structure, subnet masks and prefixes, ANDing, CIDR block sizes, network/broadcast/usable-host calculations, subnetting drills, special and private ranges, VLSM network planning and overlap checks, IPv6 notation, and a capstone office address plan. This is the core math behind Azure VNet and subnet design.

## Contents

1. [Bits and Binary](#1-bits-and-binary)
2. [IPv4 Anatomy](#2-ipv4-anatomy)
3. [The Subnet Mask](#3-the-subnet-mask)
4. [CIDR Notation](#4-cidr-notation)
5. [Inside a Subnet](#5-inside-a-subnet)
6. [Subnetting Drills](#6-subnetting-drills)
7. [Special Addresses](#7-special-addresses)
8. [Planning Networks](#8-planning-networks)
9. [IPv6 First Steps](#9-ipv6-first-steps)
10. [Capstone: The Office](#10-capstone-the-office)
11. [Subnetting cheat sheet](#subnetting-cheat-sheet)

---

## 1. Bits and Binary

### 1.1 Numbers as Bits · Beginner
**Task:** Convert 172 to 8-bit binary by hand.
**Working:** 172 = 128 + 32 + 8 + 4, so the 128, 32, 8 and 4 positions are on.
```bash
echo 10101100
```

### 1.2 Binary to Decimal · Beginner
**Task:** Convert `11000000` and `00001010` to decimal.
**Working:** 128 + 64 = 192, and 8 + 2 = 10.
```bash
echo $((2#11000000)); echo $((2#00001010))
```
Bash's `2#` prefix reads a number in base 2, which is handy for checking answers.

### 1.3 Powers of Two · Beginner
**Task:** List the eight place values of a byte.
```bash
echo "128 64 32 16 8 4 2 1"
```

### 1.4 Recap: Binary Warm-up · Easy
**Task:** Convert 240 to binary, `10101010` to decimal, and 255 to binary.
**Working:** 240 = 128 + 64 + 32 + 16. `10101010` = 128 + 32 + 8 + 2 = 170. 255 has every bit on.
```bash
printf "11110000\n170\n11111111\n"
```

---

## 2. IPv4 Anatomy

### 2.1 Four Octets · Beginner
**Task:** Convert `11000000.10101000.00000001.00000001` to dotted decimal.
```bash
echo "$((2#11000000)).$((2#10101000)).$((2#00000001)).$((2#00000001))"   # 192.168.1.1
```

### 2.2 The 32-Bit View · Beginner
**Task:** Write each octet of `10.0.0.1` as 8 bits.
```bash
printf "00001010\n00000000\n00000000\n00000001\n"
```

### 2.3 Network and Host · Beginner
**Task:** Split `192.168.7.42/24` into its network and host parts.
**Working:** /24 means the first 3 octets are network and the last octet is host.
```bash
printf "192.168.7\n42\n"
```

### 2.4 Valid or Not · Easy
**Task:** Inspect `addresses.txt` and print only the valid IPv4 addresses.
```bash
cat addresses.txt
printf "10.0.0.1\n8.8.8.8\n255.255.255.255\n"
```
A valid address has exactly 4 octets, each between 0 and 255. Entries with an octet above 255, too few or too many octets, or non-numeric parts are rejected. `255.255.255.255` is valid: it's the limited broadcast address.

### 2.5 Recap: Read an Address · Easy
**Task:** For `172.16.5.9/16`, give the network part, the host part, and 172 in binary.
```bash
printf "172.16\n5.9\n10101100\n"
```

---

## 3. The Subnet Mask

### 3.1 What a Mask Does · Beginner
**Task:** Write `255.255.255.0` in binary.
```bash
echo 11111111.11111111.11111111.00000000
```
The 1-bits mark the network portion and the 0-bits mark the host portion.

### 3.2 Mask to Prefix · Beginner
**Task:** Convert `255.0.0.0` and `255.255.255.192` to prefix lengths.
**Working:** 8 ones gives /8. 192 = `11000000`, so 24 + 2 = /26.
```bash
printf "/8\n/26\n"
```

### 3.3 Prefix to Mask · Beginner
**Task:** Convert /20 and /30 to dotted masks.
**Working:** /20 = 16 + 4 bits, and 4 bits in the third octet is 240. /30 = 24 + 6 bits, and 6 bits in the last octet is 252.
```bash
printf "255.255.240.0\n255.255.255.252\n"
```

### 3.4 ANDing It Out · Beginner
**Task:** AND `192.168.10.130` with `255.255.255.128`.
```bash
echo "192.168.10.$((130 & 128))"   # 192.168.10.128
```
A bitwise AND with the mask zeroes the host bits, which leaves the network address.

### 3.5 Recap: Mask Basics · Easy
**Task:** Convert `255.255.192.0` to a prefix, /27 to a mask, and find the network for `10.5.77.200/27`.
**Working:** 192 is 2 bits, so /18. /27 gives a last octet of 224. 200 AND 224 = 192.
```bash
printf "/18\n255.255.255.224\n10.5.77.$((200 & 224))\n"
```

---

## 4. CIDR Notation

### 4.1 Goodbye Classes · Beginner
**Task:** Give the legacy class of `10.1.1.1`, `172.30.1.1` and `200.1.2.3`.
**Working:** A first octet of 1–126 is class A, 128–191 is B, and 192–223 is C.
```bash
printf "A\nB\nC\n"
```
Classful addressing has been replaced by CIDR, but the terms still come up.

### 4.2 Reading CIDR · Beginner
**Task:** Give the network and host bits for /26 and /12.
```bash
printf "26\n$((32-26))\n12\n$((32-12))\n"   # 26 6 12 20
```

### 4.3 Block Sizes · Beginner
**Task:** Give the total number of addresses in /30, /28 and /25.
```bash
echo $((2**(32-30))); echo $((2**(32-28))); echo $((2**(32-25)))   # 4 16 128
```

### 4.4 Recap: CIDR Reading · Easy
**Task:** For /23, give the total addresses, the host bits and the mask.
**Working:** 2^9 = 512 addresses and 9 host bits. 23 = 16 + 7 bits, and 7 bits in the third octet is 254.
```bash
printf "512\n9\n255.255.254.0\n"
```

---

## 5. Inside a Subnet

### 5.1 Network Address · Beginner
**Task:** Find the network of `192.168.4.77/28`.
**Working:** A /28 is a block of 16, and 77 falls in the 64–79 block.
```bash
echo "192.168.4.$((77 / 16 * 16))"   # 192.168.4.64
```

### 5.2 Broadcast Address · Beginner
**Task:** Find the broadcast of `192.168.4.64/28`.
**Working:** Network + block size − 1 = 64 + 16 − 1 = 79.
```bash
echo 192.168.4.79
```

### 5.3 Usable Hosts · Beginner
**Task:** Give the usable hosts in /26, /29 and /24.
```bash
for p in 26 29 24; do echo $((2**(32-p) - 2)); done   # 62 6 254
```
Subtract 2 because the network and broadcast addresses can't be assigned to hosts.

### 5.4 First and Last Host · Beginner
**Task:** For `10.1.1.0/26`, give the first and last usable hosts.
**Working:** First host = network + 1. Last host = broadcast (63) − 1 = 62.
```bash
printf "10.1.1.1\n10.1.1.62\n"
```

### 5.5 Recap: Subnet Facts · Easy
**Task:** Map the subnet of `172.16.9.100/27`.
**Working:** A /27 is a block of 32, so 100 falls in 96–127.
```bash
printf "172.16.9.96\n172.16.9.127\n30\n172.16.9.97\n172.16.9.126\n"
```

---

## 6. Subnetting Drills

### 6.1 The Block Size Trick · Beginner
**Task:** List the last-octet boundaries for /26.
**Working:** Block size = 256 − 192 = 64.
```bash
echo $(seq 0 64 255)   # 0 64 128 192
```

### 6.2 Which Subnet Am I In · Beginner
**Task:** Give the network and broadcast for `192.168.1.150/26`.
```bash
printf "192.168.1.128\n192.168.1.191\n"
```

### 6.3 Splitting a /24 · Easy
**Task:** Split `10.0.5.0/24` into four equal subnets.
**Working:** Four subnets need 2 extra bits, so each is a /26 with a block of 64.
```bash
for n in 0 64 128 192; do echo "10.0.5.$n/26"; done
```

### 6.4 Splitting Again · Easy
**Task:** Split `192.168.8.64/26` into /28s.
**Working:** That gives 4 blocks of 16, starting at 64.
```bash
for n in 64 80 96 112; do echo "192.168.8.$n/28"; done
```

### 6.5 Recap: Subnetting Drills · Easy
**Task:** Give the network and broadcast for `10.10.10.77/28`, then split `172.20.0.0/24` in half.
```bash
printf "10.10.10.64\n10.10.10.79\n172.20.0.0/25\n172.20.0.128/25\n"
```

---

## 7. Special Addresses

### 7.1 Private Ranges · Easy
**Task:** Inspect `hosts.txt` and print only the RFC 1918 private addresses.
```bash
cat hosts.txt
printf "10.44.2.9\n172.20.3.3\n192.168.99.1\n"
```
The private ranges are `10.0.0.0/8`, `172.16.0.0/12` (172.16–172.31) and `192.168.0.0/16`. A trap entry such as `172.32.x.x` is **public**.

### 7.2 Loopback · Easy
**Task:** Ping `127.0.0.1` once with no output, and print `alive` or `silent`.
```bash
ping -c 1 127.0.0.1 > /dev/null 2>&1 && echo alive || echo silent
```

### 7.3 Link-Local · Beginner
**Task:** Mark each address as `apipa` (a sign that DHCP failed) or `normal`.
```bash
printf "apipa\nnormal\napipa\n"
```
A `169.254.0.0/16` address is self-assigned when no DHCP server answers. Seeing one is a classic troubleshooting clue.

### 7.4 Zero and Broadcast · Beginner
**Task:** Give the "listen on every interface" address and the local broadcast address.
```bash
printf "0.0.0.0\n255.255.255.255\n"
```

---

## 8. Planning Networks

### 8.1 Right-Sizing · Easy
**Task:** Give the smallest prefix for 50, 200 and 2 hosts.
**Working:** 50 needs 64 addresses (62 usable), so /26. 200 needs 256 (254 usable), so /24. 2 needs 4 (2 usable), so /30.
```bash
printf "/26\n/24\n/30\n"
```

### 8.2 VLSM Step by Step · Easy
**Task:** From `192.168.100.0/24`, allocate Sales (100 hosts) and then Engineering (50).
```bash
printf "192.168.100.0/25\n192.168.100.128/26\n"
```

### 8.3 Allocation Order · Easy
**Task:** Add Operations (20 hosts) to the plan.
```bash
printf "192.168.100.0/25\n192.168.100.128/26\n192.168.100.192/27\n"
```
VLSM rule: allocate **largest first**, so every block lands on a boundary that matches its size.

### 8.4 Checking for Overlap · Medium
**Task:** Find the two overlapping subnets in `plan.txt`.
```bash
cat plan.txt
printf "10.0.0.0/25\n10.0.0.64/26\n"
```
`10.0.0.0/25` covers .0–.127, and `10.0.0.64/26` (.64–.127) sits entirely inside it. Overlaps like this break routing, and Azure VNet peering rejects them too.

### 8.5 Recap: Plan a Network · Medium
**Task:** From `172.31.4.0/24`, allocate for 60, 25 and 10 hosts.
**Working:** 60 hosts → /26 (.0–.63). 25 hosts → /27 (.64–.95). 10 hosts → /28 (.96–.111).
```bash
printf "172.31.4.0/26\n172.31.4.64/27\n172.31.4.96/28\n"
```

---

## 9. IPv6 First Steps

### 9.1 Why IPv6 · Beginner
**Task:** Write the IPv6 loopback address in full form.
```bash
echo 0000:0000:0000:0000:0000:0000:0000:0001
```
The compressed form is `::1`.

### 9.2 Reading IPv6 · Beginner
**Task:** Compress `2001:0db8:0000:0000:0000:0000:0000:0001`.
```bash
echo 2001:db8::1
```
Drop the leading zeros in each group, then replace the longest run of zero groups with `::`. You can only use `::` once per address.

### 9.3 Prefixes in IPv6 · Beginner
**Task:** Give the /64 prefix of `2001:db8:aaaa:1:22:33:44:55`.
```bash
echo 2001:db8:aaaa:1::/64
```
The first 4 groups (64 bits) are the network, and the last 64 bits are the interface ID.

### 9.4 Recap: IPv6 Basics · Easy
**Task:** Compress `fe80:0000:0000:0000:0001:0000:0000:00ab`, then expand `2001:db8::5`.
```bash
printf "fe80::1:0:0:ab\n2001:0db8:0000:0000:0000:0000:0000:0005\n"
```
The three-group zero run is longer than the two-group run, so `::` replaces the first one.

---

## 10. Capstone: The Office

Scenario: design the address plan for an office with four teams from a single /23.

### 10.1 The Floor Plan · Easy
**Task:** Show the block line from `office.txt`, then the team count and the total hosts.
```bash
cat office.txt
grep "^Block:" office.txt      # Block: 10.20.0.0/23
echo 4
echo $((110 + 45 + 20 + 8))     # 183
```

### 10.2 Sizing Each Team · Easy
**Task:** Size Engineering (110), Sales (45), Support (20) and Printers (8).
```bash
printf "/25\n/26\n/27\n/28\n"
```
110 → 126 usable, 45 → 62, 20 → 30, 8 → 14.

### 10.3 Carving the Block · Medium
**Task:** Allocate the subnets largest first.
```bash
printf "10.20.0.0/25\n10.20.0.128/26\n10.20.0.192/27\n10.20.0.224/28\n"
```

### 10.4 The Router's View · Easy
**Task:** Give each subnet's gateway (its first usable address).
```bash
printf "10.20.0.1\n10.20.0.129\n10.20.0.193\n10.20.0.225\n"
```

### 10.5 Recap: Ship the Plan · Medium
**Task:** Give the unallocated space, the Sales broadcast address, and whether a future 60-host team still fits.
**Working:** A /23 has 512 addresses. 512 − (128 + 64 + 32 + 16) = 272 left. Sales (`/26` at .128) ends at .191. The free space includes all of `10.20.1.0/24`, so a /26 for 60 hosts fits.
```bash
printf "272\n10.20.0.191\nyes\n"
```

---

## Subnetting cheat sheet

| Prefix | Mask (last octet) | Block size | Usable hosts |
| ---: | ---: | ---: | ---: |
| /24 | 0 | 256 | 254 |
| /25 | 128 | 128 | 126 |
| /26 | 192 | 64 | 62 |
| /27 | 224 | 32 | 30 |
| /28 | 240 | 16 | 14 |
| /29 | 248 | 8 | 6 |
| /30 | 252 | 4 | 2 |

- **Network** = address AND mask. **Broadcast** = network + block − 1.
- **Usable** = 2^(32 − prefix) − 2.
- **VLSM:** allocate largest first, then check that no two ranges overlap.
- **Azure note:** Azure reserves 5 addresses per subnet (not 2), so a /29 gives only 3 usable addresses in a VNet.
