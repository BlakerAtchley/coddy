# Coddy Hands-On Training Portfolio

![Computer Networking Path](https://img.shields.io/badge/Computer%20Networking%20Path-Complete-2ea44f)
![Certificates](https://img.shields.io/badge/Certificates-8-1f6feb)
![Lessons](https://img.shields.io/badge/Path%20lessons-282-1f6feb)
![Challenges](https://img.shields.io/badge/Graded%20challenges-280-1f6feb)
![Cybersecurity](https://img.shields.io/badge/Cybersecurity-In%20progress-d29922)

Verified, hands-on training completed on [Coddy](https://coddy.tech) in a live, browser-based Linux terminal. Every lesson ends in a graded challenge, so each certificate reflects commands I actually ran, not videos I watched.

This repo is part of my path into **Cloud Security Engineering**. The full plan lives in my [roadmap](https://github.com/BlakerAtchley/roadmap).

---

## Contents

- [Highlights](#highlights)
- [Certificates](#certificates)
- [Computer Networking learning path](#computer-networking-learning-path-complete)
- [Projects and capstones](#projects-and-capstones)
- [Cybersecurity track](#cybersecurity-track-in-progress)
- [Skills demonstrated](#skills-demonstrated)
- [Full curriculum](#full-curriculum)

---

## Highlights

- **Completed the Computer Networking learning path:** 6 courses, 282 lessons and 280 graded challenges
- **8 verifiable certificates** across Linux, networking, Docker and cybersecurity
- **Hands-on projects:** Bash backup automation, log analysis, a containerized app, a subnet plan for an office and a broken branch network brought back online
- **Now training in cybersecurity:** incident investigation, evidence handling and recovery

---

## Certificates

Click a certificate name to verify it on Coddy. PDF copies are in [`certificates/`](certificates/).

| Date | Certificate | Type | PDF |
| --- | --- | --- | --- |
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

The path teaches networking in the order a network engineer learns it: terminal fluency first, then the OSI model, IP addressing, DNS and ports, reachability tools, HTTP and TLS, LAN troubleshooting and container networking. Each stage introduces the concepts, then drills them with terminal commands, graded challenges and lab scenarios.

| Step | Course | Lessons | Challenges | What I can do now |
| --- | --- | ---: | ---: | --- |
| 1 | Terminal Fundamentals | 82 | 80 | Navigate and manage a Linux filesystem; use wildcards, redirection, pipes and `grep`; set permissions with `chmod`; configure the environment; write Bash scripts with input, conditions and loops |
| 2 | The OSI Model | 32 | 32 | Explain all seven layers and the TCP/IP model; read MAC addresses and ARP; describe routing, TTL and ICMP; compare TCP and UDP; trace encapsulation and a packet's end-to-end journey |
| 3 | IP Addressing and Subnetting | 46 | 46 | Convert binary; read IPv4 and CIDR; find network, broadcast and usable host ranges; subnet a /24; plan networks with VLSM and check for overlap; read IPv6 prefixes |
| 4 | Networking with the Terminal | 47 | 47 | Resolve names with DNS; work with ports and sockets; test reachability with `ping`; write HTTP requests by hand; send data and debug with `curl`; diagnose TLS and authentication failures |
| 5 | LAN Labs | 35 | 35 | Build and address small networks; read ARP and switch tables; configure gateways and routing; find and fix wrong masks, missing gateways, address clashes and routing faults |
| 6 | Introduction to Docker | 40 | 40 | Pull, tag, run, inspect and manage containers; write Dockerfiles with `COPY`, `WORKDIR` and `CMD`; copy files in and out; use volumes and container networks |
| | **Total** | **282** | **280** | |

---

## Projects and capstones

| Project | Course | What I built |
| --- | --- | --- |
| Backup Script | Terminal Fundamentals | Bash script that prompts for a source directory, creates a timestamped backup folder (`date +%Y%m%d_%H%M%S`), copies the files and confirms each step |
| Log Analyzer | Terminal Fundamentals | Read a log file, filtered out the errors, counted occurrences and generated a report using command-line tools |
| Organize Photos | Terminal Fundamentals | Built a folder structure, moved and copied files with `mkdir`, `mv`, `cp` and `rm`, then verified the final layout |
| Capstone: The Office | IP Addressing and Subnetting | Sized each team's subnet, carved one address block with VLSM and checked the plan from the router's view |
| Capstone: Fix the Branch | LAN Labs | Surveyed a broken branch network, fixed an address clash, wrong masks and gateways, then proved it worked |
| Greeting App | Introduction to Docker | Wrote a Dockerfile, built and ran the image, personalized the app and shipped a tagged version |

**Final challenges passed**

- **Bash:** file organizer by type, word frequency counter, system health checker
- **Docker:** distro detective, full build pipeline, runtime file drop

---

## Cybersecurity track (in progress)

A scenario-based track where I work as the first security analyst for a small company.

| Section | Status | Focus |
| --- | --- | --- |
| Security Fundamentals | ✅ Complete | Investigating alerts, reviewing access, strengthening passwords, handling email attacks, verifying authorization, containing and recovering from an incident |
| Investigating a Breach | 🔄 15 / 20 lessons | Tracing suspicious programs to their source, saving evidence, removing persistence, proving the recovery |
| Stopping the Spread | ⏳ Next | Tracing suspicious connections, containing affected computers, restricting access |

**Also in progress:** Git version control (27 / 58 lessons)

---

## Skills demonstrated

| Area | Skills |
| --- | --- |
| Linux | Filesystem navigation, wildcards, `grep`, pipes and redirection, permissions and ownership, environment variables, Bash scripting |
| Networking | OSI and TCP/IP models, MAC and ARP, routing tables, TCP vs. UDP, ports and sockets, encapsulation |
| Addressing | Binary, IPv4, subnet masks, CIDR, VLSM network planning, private and special ranges, IPv6 basics |
| Troubleshooting | `ping` and ICMP, DNS lookups, HTTP status codes, `curl` debugging, TLS failures, LAN fault isolation |
| Containers | Docker images and containers, Dockerfiles, volumes, container networks |
| Security | Alert triage, access review, phishing handling, incident containment and recovery (in progress) |

---

## Full curriculum

Everything below is completed. Expand a step to see the chapters and lessons.

<details>
<summary><b>Step 1: Terminal Fundamentals</b> (82 lessons, 80 challenges)</summary>

Master the command line. Every networking tool in the path is a terminal command, so this stage builds the foundation.

| Chapter | Lessons |
| --- | --- |
| 01. Introduction | What is the terminal, your first command, comments |
| 02. Navigation | `pwd`, listing files, changing directory, absolute vs. relative paths, home and root directories |
| 03. Files | Create, read, copy, move and rename, delete |
| 04. Directories | Create, copy, move and rename, delete |
| 05. Organize Photos Project | Project overview, creating the structure, moving the files, cleaning up |
| 06. Wildcards and Patterns | `*`, `?`, bracket wildcards, combining wildcards |
| 07. File Content | `head` and `tail`, `wc`, `sort`, `uniq`, `grep` basics and flags |
| 08. Redirection | Standard output, overwrite, append, standard input, standard error |
| 09. Piping | Pipes, chaining two and multiple commands, piping into `grep` |
| 10. Log Analyzer Project | Viewing the log, filtering errors, counting occurrences, generating a report |
| 11. Permissions | Reading permissions, `chmod` with numbers and symbols, file ownership |
| 12. Environment | Environment variables, `PATH`, setting and exporting variables, the profile file |
| 13. Shell Scripting Basics | Creating and running scripts, variables, user input, `if`, `for` and `while` loops |
| 14. Backup Script Project | Source path, backup folder, copying files, timestamps, success message |
| 15. Final Challenges | File organizer by type, word frequency counter, system health checker |

</details>

<details>
<summary><b>Step 2: The OSI Model</b> (32 lessons, 32 challenges)</summary>

How networks really work, one layer at a time: the seven OSI layers, the TCP/IP model, and how data is encapsulated as it crosses a network.

| Chapter | Lessons |
| --- | --- |
| 01. Why Layers | The layering idea, the seven layers, numbers and names |
| 02. The Physical Layer | Bits on a wire, transmission time, duplex and collisions |
| 03. Data Link and MACs | Frames and MACs, reading a MAC, ARP |
| 04. The Network Layer | IP and TTL, `ping` and ICMP, routing tables |
| 05. Transport: TCP and UDP | Ports, the TCP handshake, TCP vs. UDP, sockets in action |
| 06. The Upper Layers | Session and presentation, the application layer, watching the layers |
| 07. Encapsulation | Wrapping data, headers, reading a capture |
| 08. The Journey of a Packet | A click happens, crossing networks, arriving and climbing the stack |

</details>

<details>
<summary><b>Step 3: IP Addressing and Subnetting</b> (46 lessons, 46 challenges)</summary>

IP addresses from the bits up: the math and reasoning to design, divide and troubleshoot network ranges.

| Chapter | Lessons |
| --- | --- |
| 01. Bits and Binary | Numbers as bits, binary to decimal, powers of two |
| 02. IPv4 Anatomy | Four octets, the 32-bit view, network and host, valid or not |
| 03. The Subnet Mask | What a mask does, mask to prefix, prefix to mask, ANDing |
| 04. CIDR Notation | Classless addressing, reading CIDR, block sizes |
| 05. Inside a Subnet | Network address, broadcast address, usable hosts, first and last host |
| 06. Subnetting Drills | The block size trick, which subnet am I in, splitting a /24 and splitting again |
| 07. Special Addresses | Private ranges, loopback, link-local, zero and broadcast |
| 08. Planning Networks | Right-sizing, VLSM step by step, allocation order, checking for overlap |
| 09. IPv6 First Steps | Why IPv6, reading IPv6, IPv6 prefixes |
| 10. Capstone: The Office | Floor plan, sizing each team, carving the block, the router's view |

</details>

<details>
<summary><b>Step 4: Networking with the Terminal</b> (47 lessons, 47 challenges)</summary>

How machines actually talk to each other, tested from the command line against real hosts.

| Chapter | Lessons |
| --- | --- |
| 01. How Networks Talk | Networks, clients and servers, IP addresses, your first request |
| 02. Names and DNS | Why names exist, the hosts file, looking up a name, record types |
| 03. Ports and Sockets | Ports, well-known ports, localhost, listening vs. connecting |
| 04. Is It Reachable? | ICMP and `ping`, reading ping output, when ping lies, refused vs. timed out |
| 05. HTTP by Hand | Request anatomy, methods, status codes, response headers, writing a response |
| 06. curl in Depth | Headers, saving responses, custom headers, status and timing, exit codes |
| 07. Sending Data | POST with `-d`, form encoding, sending JSON, PUT and DELETE |
| 08. HTTPS and TLS | Why HTTPS, scheme and port, certificates and trust, when TLS fails |
| 09. Auth and Debugging | API keys and authentication, user agent, timeouts and retries, reading a failure |

</details>

<details>
<summary><b>Step 5: LAN Labs</b> (35 lessons, 35 challenges)</summary>

Build, address and troubleshoot small networks in a terminal lab with hosts, switches, routers, gateways and routing tables.

| Chapter | Lessons |
| --- | --- |
| 01. Meet the Lab | Your first lab, cables, why wires are not enough |
| 02. Addresses on the LAN | Addressing a host, the first ping, what ARP learned, how the switch learns |
| 03. Masks Matter | Wrong mask and no reply, same switch but another subnet, diagnosing the pair |
| 04. Routers and Gateways | Two networks, the default gateway, return paths, routing tables |
| 05. Troubleshooting I | Unplugged host, missing gateway, typo in the mask |
| 06. Troubleshooting II | Duplicate addresses, the network address trap, gateway on the wrong subnet |
| 07. Growing the Office | Planning, bringing Sales and Engineering online, gateways everywhere |
| 08. Capstone: Fix the Branch | Surveying the damage, fixing the address clash, masks and gateways, proving it works |

</details>

<details>
<summary><b>Step 6: Introduction to Docker</b> (40 lessons, 40 challenges)</summary>

Docker from the command line: how containers are built and run, get addresses, talk to each other and reach the outside world.

| Chapter | Lessons |
| --- | --- |
| 01. Introduction | What Docker is, images vs. containers |
| 02. Images | Pulling, listing and tagging images |
| 03. Running Containers | Running commands, different distros, auto-remove containers |
| 04. Managing Containers | Naming, detached mode, listing, stopping and removing |
| 05. Inspecting Containers | Logs, inspecting details, executing commands inside a container |
| 06. Building Images | Dockerfiles, building an image, the `CMD` instruction |
| 07. Files in Images | The `COPY` and `WORKDIR` instructions |
| 08. Copying Files | Copying into and out of a container |
| 09. Volumes and Networks | Volumes, container networks |
| 10. Greeting App Project | Writing the Dockerfile, build and run, personalizing, shipping a tagged version |
| 11. Final Challenges | Distro detective, full build pipeline, runtime file drop |

</details>

---

_Last updated: 2026-11-16_
