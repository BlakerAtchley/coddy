# Coddy Hands-On Training Portfolio

![Computer Networking Path](https://img.shields.io/badge/Computer%20Networking%20Path-Complete-2ea44f)
![Certificates](https://img.shields.io/badge/Certificates-9-1f6feb)
![Lessons](https://img.shields.io/badge/Lessons%20completed-345-1f6feb)
![Challenges](https://img.shields.io/badge/Graded%20challenges-343-1f6feb)
![Cybersecurity](https://img.shields.io/badge/Cybersecurity-2%20of%203%20sections-d29922)

Verified, hands-on training completed on [Coddy](https://coddy.tech) in a live, browser-based Linux terminal. Every lesson ends in a graded challenge, so each certificate reflects commands I actually ran, not videos I watched.

This repo is part of my path into **Cloud Security Engineering** (Azure focus). The full plan lives in my [roadmap](https://github.com/BlakerAtchley/roadmap).

---

## Contents

- [Highlights](#highlights)
- [Course write-ups](#course-write-ups)
- [Certificates](#certificates)
- [Computer Networking learning path](#computer-networking-learning-path-complete)
- [Projects and capstones](#projects-and-capstones)
- [Cybersecurity track](#cybersecurity-track)
- [Skills demonstrated](#skills-demonstrated)

---

## Highlights

- **Completed the Computer Networking learning path:** 6 courses, 282 lessons and 280 graded challenges
- **Completed 2 cybersecurity sections:** 63 scenario-based incident-response challenges
- **9 verifiable certificates** across Linux, networking, Docker and cybersecurity
- **Hands-on projects:** Bash backup automation, log analysis, a containerized app, a VLSM subnet plan for an office, a broken branch network brought back online, and full endpoint and account incident-response cases

---

## Course write-ups

Each course has its own detailed write-up covering every chapter and every graded challenge: the task, the commands that solve it, and what those commands do.

| Course | Lessons | Challenges | Write-up |
| --- | ---: | ---: | --- |
| Terminal Fundamentals | 82 | 80 | [Terminal-Fundamentals.md](Terminal-Fundamentals.md) |
| The OSI Model | 32 | 32 | [The-OSI-Model.md](The-OSI-Model.md) |
| IP Addressing and Subnetting | 46 | 46 | [IP-Addressing-and-Subnetting.md](IP-Addressing-and-Subnetting.md) |
| Networking with the Terminal | 47 | 47 | [Networking-with-the-Terminal.md](Networking-with-the-Terminal.md) |
| LAN Labs | 35 | 35 | [LAN-Labs.md](LAN-Labs.md) |
| Introduction to Docker | 40 | 40 | [Introduction-to-Docker.md](Introduction-to-Docker.md) |
| Cybersecurity: Security Fundamentals | 43 | 43 | [Cybersecurity-Security-Fundamentals.md](Cybersecurity-Security-Fundamentals.md) |
| Cybersecurity: Investigating a Breach | 20 | 20 | [Cybersecurity-Investigating-a-Breach.md](Cybersecurity-Investigating-a-Breach.md) |
| **Total** | **345** | **343** | |

> Coddy's terminal grades each challenge live but doesn't keep a copy of what was typed. The commands in the write-ups are reference solutions I documented after completing each challenge.

---

## Certificates

Click a certificate name to verify it on Coddy. PDF copies are in [`certificates/`](certificates/).

| Date | Certificate | Type | PDF |
| --- | --- | --- | --- |
| 2026-11-16 | [Cybersecurity: Investigating a Breach](https://coddy.tech/certifications/tj6FfM-cybersecurity-cFAQiR) | Section | — |
| 2026-11-15 | [Cybersecurity: Security Fundamentals](https://coddy.tech/certifications/tj6FfM-cybersecurity-l0kMWP) | Section | [PDF](certificates/2026-11-15_cybersecurity-security-fundamentals.pdf) |
| 2026-11-14 | **[Computer Networking](https://coddy.tech/certifications/tj6FfM-path-networking)** | **Learning path (6 courses, 282 lessons)** | [PDF](certificates/2026-11-14_computer-networking-path.pdf) |
| 2026-11-14 | [Terminal: Introduction to Docker](https://coddy.tech/certifications/tj6FfM-terminal-fhs4B2) | Section | [PDF](certificates/2026-11-14_introduction-to-docker.pdf) |
| 2026-11-14 | [LAN Labs](https://coddy.tech/certifications/tj6FfM-AYJDHQ) | Course | [PDF](certificates/2026-11-14_lan-labs.pdf) |
| 2026-11-10 | [Networking with the Terminal](https://coddy.tech/certifications/tj6FfM-qnnXQ5) | Course | [PDF](certificates/2026-11-10_networking-with-the-terminal.pdf) |
| 2026-11-08 | [IP Addressing and Subnetting](https://coddy.tech/certifications/tj6FfM-LDYK7g) | Course | [PDF](certificates/2026-11-08_ip-addressing-and-subnetting.pdf) |
| 2026-11-07 | [The OSI Model](https://coddy.tech/certifications/tj6FfM-cZAW4t) | Course | [PDF](certificates/2026-11-07_the-osi-model.pdf) |
| 2026-11-05 | [Terminal Fundamentals](https://coddy.tech/certifications/tj6FfM-terminal-JRk84c) | Section | [PDF](certificates/2026-11-05_terminal-fundamentals.pdf) |

---

## Computer Networking learning path (complete)

The path teaches networking in the order a network engineer learns it: terminal fluency first, then the OSI model, IP addressing, DNS and ports, reachability tools, HTTP and TLS, LAN troubleshooting and container networking.

| Step | Course | Lessons | Challenges | What I can do now |
| --- | --- | ---: | ---: | --- |
| 1 | [Terminal Fundamentals](Terminal-Fundamentals.md) | 82 | 80 | Navigate and manage a Linux filesystem; use wildcards, redirection, pipes and `grep`; set permissions with `chmod`; configure the environment; write Bash scripts with input, conditions and loops |
| 2 | [The OSI Model](The-OSI-Model.md) | 32 | 32 | Explain all seven layers and the TCP/IP model; read MAC addresses and ARP; describe routing, TTL and ICMP; compare TCP and UDP; trace encapsulation and a packet's end-to-end journey |
| 3 | [IP Addressing and Subnetting](IP-Addressing-and-Subnetting.md) | 46 | 46 | Convert binary; read IPv4 and CIDR; find network, broadcast and usable host ranges; subnet a /24; plan networks with VLSM and check for overlap; read IPv6 prefixes |
| 4 | [Networking with the Terminal](Networking-with-the-Terminal.md) | 47 | 47 | Resolve names with DNS; work with ports and sockets; test reachability with `ping`; write HTTP requests by hand; send data and debug with `curl`; diagnose TLS and authentication failures |
| 5 | [LAN Labs](LAN-Labs.md) | 35 | 35 | Build and address small networks; read ARP and switch tables; configure gateways and routing; find and fix wrong masks, missing gateways, address clashes and routing faults |
| 6 | [Introduction to Docker](Introduction-to-Docker.md) | 40 | 40 | Pull, tag, run, inspect and manage containers; write Dockerfiles with `COPY`, `WORKDIR` and `CMD`; copy files in and out; use volumes and container networks |
| | **Total** | **282** | **280** | |

---

## Projects and capstones

| Project | Course | What I built |
| --- | --- | --- |
| Organize Photos | Terminal Fundamentals | Built a folder structure, moved and copied files with `mkdir`, `mv`, `cp` and `rm`, then verified the final layout |
| Log Analyzer | Terminal Fundamentals | Filtered and counted errors in a server log and generated a report with pipes, redirection and command substitution |
| Backup Script | Terminal Fundamentals | Bash script that prompts for a source directory, creates a timestamped backup folder (`date +%Y%m%d_%H%M%S`), copies the files and confirms each step |
| Capstone: The Office | IP Addressing and Subnetting | Sized four teams' subnets, carved a /23 with VLSM, assigned gateways and checked the remaining space |
| Growing the Office | LAN Labs | Built a two-department network with a core router, per-department subnets, gateways and a shared printer |
| Capstone: Fix the Branch | LAN Labs | Surveyed a broken branch network, fixed an address clash, wrong masks and gateways, then proved it worked |
| Greeting App | Introduction to Docker | Wrote a Dockerfile, built and ran the image, personalized the app and shipped a tagged version |
| The Night Alert | Security Fundamentals | Full account-compromise response: blocked the source, removed a rogue admin account and forwarding rule, reset credentials and restored wrongly blocked users |
| The Stock Room | Investigating a Breach | Full endpoint response: preserved evidence, stopped and quarantined malware, removed startup persistence, reconnected the host and verified through a restart |

**Final challenges passed**

- **Bash:** file organizer by type, word frequency counter, system health checker
- **Docker:** distro detective, full build pipeline, runtime file drop

---

## Cybersecurity track

A scenario-based track where I work as the first security analyst for a small company, handling tickets through a security console.

| Section | Status | Focus |
| --- | --- | --- |
| [Security Fundamentals](Cybersecurity-Security-Fundamentals.md) | ✅ Complete (43/43) | Alert triage, MFA, phishing, access control, password policy, email attacks, rules of engagement, incident response |
| [Investigating a Breach](Cybersecurity-Investigating-a-Breach.md) | ✅ Complete (20/20) | Process and parent tracing, SHA-256 checks, evidence preservation, quarantine, startup persistence, timelines, recovery |
| Stopping the Spread | 🔄 In progress | Tracing suspicious connections, containing affected computers, restricting access |

**Also in progress:** Git version control (27 / 58 lessons)

---

## Skills demonstrated

| Area | Skills |
| --- | --- |
| Linux | Filesystem navigation, wildcards, `grep`, pipes and redirection, permissions and ownership, environment variables, Bash scripting |
| Networking | OSI and TCP/IP models, MAC and ARP, routing tables, TCP vs. UDP, ports and sockets, encapsulation |
| Addressing | Binary, IPv4, subnet masks, CIDR, VLSM network planning, private and special ranges, IPv6 basics |
| Troubleshooting | `ping` and ICMP, DNS lookups, HTTP status codes, `curl` debugging and exit codes, TLS failures, LAN fault isolation |
| Containers | Docker images and containers, Dockerfiles, volumes, container networks |
| Identity & access | MFA, least privilege, separation of duties, joiner/mover/leaver handling, password policy, session revocation |
| Email security | Phishing and look-alike domains, Reply-To and header analysis, BEC, sender blocks, malicious mailbox rules |
| Incident response | Containment, evidence preservation, process/hash analysis, persistence removal, recovery, reporting |

---

_Last updated: 2026-11-19_
