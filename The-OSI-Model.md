# The OSI Model

**Coddy Computer Networking path, Step 2** · 8 chapters · 32 lessons · 32 graded challenges · Certificate earned 2026-11-07 ([verify](https://coddy.tech/certifications/tj6FfM-cZAW4t))

[← Back to portfolio summary](README.md)

> **About the commands below:** Coddy's browser terminal grades each challenge live but does not keep a copy of what was typed. The commands here are reference solutions I wrote afterward for each challenge I passed. Most challenges in this course check a concept by having you print the answer, so the solution is the `echo`/`printf` that produces the graded output, along with the reasoning behind it.

## What this course covers

How data moves through a network one layer at a time: the seven OSI layers and their numbers, physical-layer math (bits vs. bytes, transfer time, duplex), MAC addresses and ARP, IP, TTL, ICMP and longest-prefix routing, TCP vs. UDP, ports and sockets, the upper layers, encapsulation and PDUs, and the full end-to-end journey of a web request.

## Contents

1. [Why Layers](#1-why-layers)
2. [The Physical Layer](#2-the-physical-layer)
3. [Data Link and MACs](#3-data-link-and-macs)
4. [The Network Layer](#4-the-network-layer)
5. [Transport: TCP and UDP](#5-transport-tcp-and-udp)
6. [The Upper Layers](#6-the-upper-layers)
7. [Encapsulation](#7-encapsulation)
8. [The Journey of a Packet](#8-the-journey-of-a-packet)
9. [Quick reference](#quick-reference)

---

## 1. Why Layers

### 1.1 The Layering Idea · Beginner
**Task:** Name the process of each layer wrapping the data it receives from the layer above.
```bash
echo encapsulation
```
Each layer adds its own header around the payload handed down from above. The receiver reverses the process (de-encapsulation).

### 1.2 Seven Layers · Beginner
**Task:** List all seven layers from layer 7 down to layer 1.
```bash
printf "Application\nPresentation\nSession\nTransport\nNetwork\nData Link\nPhysical\n"
```

### 1.3 Numbers and Names · Beginner
**Task:** Give the layer numbers for Network, Transport and Application.
```bash
printf "3\n4\n7\n"
```
These three come up most in real work: L3 = IP, L4 = TCP/UDP, L7 = HTTP. Mnemonic, bottom-up: *Please Do Not Throw Sausage Pizza Away*.

### 1.4 Recap: The Stack · Easy
**Task:** Name layers 2, 5 and 1.
```bash
printf "Data Link\nSession\nPhysical\n"
```

---

## 2. The Physical Layer

### 2.1 Bits on a Wire · Beginner
**Task:** For a 1 Gbps line, give the speed in Mbps and the maximum rate in megabytes per second.
```bash
echo 1000          # 1 Gbps = 1000 Mbps
echo $((1000 / 8)) # 8 bits per byte -> 125 MB/s
```
Link speeds are quoted in **bits**, file sizes in **bytes**. That's why downloads seem about 8× slower than the plan's headline number.

### 2.2 How Long Will It Take · Beginner
**Task:** Work out the best-case time to move 100 MB over a 100 Mbps link.
```bash
echo $((100 * 8 / 100))   # 800 megabits / 100 Mbps = 8 seconds
```

### 2.3 Duplex and Collisions · Beginner
**Task:** Give the duplex mode of a hub, then of a switch.
```bash
printf "half\nfull\n"
```
A hub repeats every signal to every port, so all devices share one collision domain. A switch gives each port its own full-duplex link.

---

## 3. Data Link and MACs

### 3.1 Frames and MACs · Beginner
**Task:** Give the length of a MAC address in bytes and in bits.
```bash
printf "6\n48\n"
```

### 3.2 Reading a MAC · Beginner
**Task:** Find the OUI (vendor prefix) of `3c:5a:b4:12:34:56`.
```bash
echo 3c:5a:b4
# or derive it: echo 3c:5a:b4:12:34:56 | cut -d: -f1-3
```
The first 3 bytes identify the NIC manufacturer, which is a handy way to identify unknown devices on a network.

### 3.3 ARP · Easy
**Task:** Pull the entry for `192.168.1.7` out of `arp_table.txt`.
```bash
grep "^192.168.1.7 " arp_table.txt
```
Output: `192.168.1.7 aa:bb:cc:00:00:07`. Anchoring with `^` and a trailing space avoids also matching `192.168.1.70`. On a live system you'd use `arp -a` or `ip neigh`.

### 3.4 Recap: Layer Two · Easy
**Task:** Give the OUI of `de:ad:be:ef:00:01`, the L2 broadcast address, and the protocol that maps IP to MAC.
```bash
printf "de:ad:be\nff:ff:ff:ff:ff:ff\nARP\n"
```

---

## 4. The Network Layer

### 4.1 IP and TTL · Beginner
**Task:** A packet leaves with TTL 64 and crosses 3 routers. Give the TTL on arrival.
```bash
echo $((64 - 3))   # 61
```
Each router decrements TTL by 1 and drops the packet at 0, which stops routing loops. `traceroute` relies on this behavior.

### 4.2 Ping and ICMP · Easy
**Task:** Ping localhost once with no ping output, and print `ok` or `fail`.
```bash
ping -c 1 127.0.0.1 > /dev/null 2>&1 && echo ok || echo fail
```
The script relies on ping's **exit status** (0 = reply received), not its text. `-c 1` sends exactly one ICMP echo request.

### 4.3 Routing Tables · Easy
**Task:** Using the routes A = `0.0.0.0/0`, B = `10.0.0.0/8` and C = `10.1.0.0/16`, pick the gateway for `10.1.2.3`, `10.9.9.9` and `8.8.8.8`.
```bash
printf "C\nB\nA\n"
```
**Longest prefix wins:** `10.1.2.3` matches all three routes, and `/16` is the most specific. `10.9.9.9` only matches `/8` and the default. `8.8.8.8` only matches the default.

### 4.4 Recap: Layer Three · Easy
**Task:** Give the TTL after 5 hops from 64, the gateway for `10.1.9.9`, and the protocol behind ping.
```bash
echo $((64 - 5)); echo C; echo ICMP
```

---

## 5. Transport: TCP and UDP

### 5.1 Ports · Beginner
**Task:** Give the highest possible port and the number of well-known ports.
```bash
printf "65535\n1024\n"
```
Ports are 16-bit numbers (0–65535). Ports 0–1023 are "well-known" (for example, 22 SSH, 80 HTTP, 443 HTTPS).

### 5.2 The TCP Handshake · Beginner
**Task:** List the three handshake steps in order.
```bash
printf "SYN\nSYN-ACK\nACK\n"
```

### 5.3 TCP vs UDP · Beginner
**Task:** Pick the protocol for a live video call, a zip download and a DNS lookup.
```bash
printf "UDP\nTCP\nUDP\n"
```
The deciding question is whether a late packet is still useful. A late video frame is useless (UDP), a download must be complete (TCP), and DNS is a tiny one-shot query (UDP).

### 5.4 Sockets in Action · Medium
**Task:** Start a server on port 9210, wait for it, then check whether the port answers.
```bash
python3 -m http.server 9210 > /dev/null 2>&1 &
for i in 1 2 3 4 5; do nc -z 127.0.0.1 9210 && break; sleep 1; done
nc -z 127.0.0.1 9210 && echo open || echo closed
```
`&` backgrounds the server. The loop retries because servers take a moment to bind. `nc -z` only checks whether a TCP connection opens, without sending data.

### 5.5 Recap: Layer Four · Easy
**Task:** Give the default ports for HTTP, HTTPS and DNS.
```bash
printf "80\n443\n53\n"
```

---

## 6. The Upper Layers

### 6.1 Session and Presentation · Beginner
**Task:** Give the OSI layer for encryption and for dialog management.
```bash
printf "6\n5\n"
```
In OSI theory, encryption and format translation belong to layer 6 (Presentation) and dialog control to layer 5 (Session). In practice TLS and the applications themselves cover these jobs, but exams still ask for the theory placement.

### 6.2 The Application Layer · Beginner
**Task:** Name the L7 protocol for web pages, server-to-server email, and name resolution.
```bash
printf "HTTP\nSMTP\nDNS\n"
```

### 6.3 Watching the Layers · Medium
**Task:** Start a local server on 9220 and print only the HTTP status code curl gets back.
```bash
python3 -m http.server 9220 > /dev/null 2>&1 &
for i in 1 2 3 4 5; do nc -z 127.0.0.1 9220 && break; sleep 1; done
curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:9220/
```
Output: `200`. A single curl call exercises name/IP handling, a TCP connection (L4) and an HTTP request and response (L7).

### 6.4 Recap: Top of the Stack · Easy
**Task:** Give the layer for HTTP, TLS (in theory) and TCP.
```bash
printf "7\n6\n4\n"
```

---

## 7. Encapsulation

### 7.1 Wrapping Data · Beginner
**Task:** List the PDU names in the order the sender creates them.
```bash
printf "data\nsegment\npacket\nframe\nbits\n"
```

### 7.2 Headers Everywhere · Beginner
**Task:** Name the header that carries MACs, IPs and ports.
```bash
printf "Ethernet\nIP\nTCP\n"
```
On the wire the order is `Ethernet | IP | TCP | data`, outermost first.

### 7.3 Reading a Capture · Easy
**Task:** In `capture.txt`, find the packet whose TCP destination port is 53.
```bash
grep "tcp [0-9]*>53$" capture.txt
```
Output: `3 eth aa:00:00:01>aa:00:00:02 ip 10.0.0.5>10.0.0.53 tcp 51002>53`. The `>53$` anchor matches the **destination** port only, not an IP that happens to contain `53`.

### 7.4 Recap: Follow the Wrap · Easy
**Task:** Give the PDU for layers 4, 3 and 2.
```bash
printf "segment\npacket\nframe\n"
```

---

## 8. The Journey of a Packet

### 8.1 A Click Happens · Beginner
**Task:** Put the protocols that run when you click a link in order.
```bash
printf "DNS\nTCP\nHTTP\n"
```
The name must resolve first, then the TCP handshake opens a connection, and only then does the HTTP request go out.

### 8.2 Crossing the Networks · Beginner
**Task:** Say which address changes at every hop and which stays the same end to end.
```bash
printf "MAC\nIP\n"
```
MACs are per hop and IPs are end to end. Each router strips the frame and builds a new one, and only the TTL changes in the IP header.

### 8.3 Arriving and Climbing · Beginner
**Task:** Name the services that wake for ports 443, 22 and 53.
```bash
printf "HTTPS\nSSH\nDNS\n"
```
The destination port tells the receiving host which program gets the data (demultiplexing).

### 8.4 Recap: End to End · Medium
**Task:** Give the routing layer, the PDU that is on the wire, the header with ports, and the protocol that turns names into addresses.
```bash
printf "3\nbits\nTCP\nDNS\n"
```

---

## Quick reference

| Layer | Name | PDU | Addressing / examples |
| ---: | --- | --- | --- |
| 7 | Application | data | HTTP, DNS, SMTP, SSH |
| 6 | Presentation | data | Encoding, compression, encryption (theory) |
| 5 | Session | data | Dialog setup/teardown |
| 4 | Transport | segment / datagram | Ports; TCP, UDP |
| 3 | Network | packet | IP addresses, TTL, routing, ICMP |
| 2 | Data Link | frame | MAC addresses, ARP, switches |
| 1 | Physical | bits | Copper, fiber, radio; duplex |
