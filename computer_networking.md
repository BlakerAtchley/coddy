# Computer Networking Study Path

A structured, hands-on learning roadmap for building practical networking skills from the command line to real-world troubleshooting and Docker networking.

## Overview

This path is designed to teach networking in the same order a network engineer typically learns it: terminal fluency first, then the OSI model, IP addressing, DNS and ports, reachability tools, HTTP and TLS, and finally LAN troubleshooting and container networking.

The curriculum emphasizes practice, not memorization. Each stage introduces core concepts, then reinforces them through terminal commands, challenge exercises, and lab-style scenarios.

## Learning Objectives

By the end of this path, learners should be able to:

- Use essential Linux terminal commands confidently
- Explain the OSI and TCP/IP networking models
- Work with IPv4 addresses, masks, CIDR, and subnetting
- Understand DNS, ports, sockets, and common troubleshooting tools
- Use `ping`, `curl`, and protocol inspection to debug connectivity and HTTP issues
- Build and troubleshoot small local area networks (LANs)
- Understand Docker images, containers, volumes, networks, and how they connect

## Course Progression

| Step | Focus | Lessons | Challenges | Outcome |
| --- | --- | ---: | ---: | --- |
| 1 | Fundamentals | 82 | 80 | Build command-line confidence and folder/file mastery |
| 2 | OSI and networking layers | 32 | 32 | Understand how data moves through a network |
| 3 | IP addressing and subnetting | 46 | 46 | Work with IPv4, masks, CIDR, and VLSM |
| 4 | Hosts, DNS, ports, HTTP, TLS | 47 | 47 | Debug real communication between machines |
| 5 | LAN lab and troubleshooting | 35 | 35 | Configure and fix small network environments |
| 6 | Docker networking | 40 | 40 | Learn container fundamentals and network behavior |

---

## Step 1: Fundamentals

### Purpose
Master the command line with essential tools and core techniques. Every networking tool in this path is ultimately a terminal command, so this stage establishes the foundation.

### Prerequisites
- No prior knowledge required

### Key topics
- Filesystem navigation and permissions
- File and directory management
- Wildcards and pattern matching
- Reading logs and filtering results
- Redirection and piping
- Environment variables and shell scripting
- Backup automation project and final challenges

### Syllabus

#### 01. Introduction
- What is the terminal
- Your first command
- Comments

#### 02. Navigation
- Print working directory
- List files
- Change directory
- Absolute vs relative paths
- Home and root directories
- Recap: find your way

#### 03. Files
- Create a file
- Read a file
- Copy a file
- Move and rename a file
- Delete a file
- Recap: file operations

#### 04. Directories
- Create a directory
- Copy a directory
- Move and rename a directory
- Delete a directory
- Recap: directory operations

#### 05. Organize Photos Project
- Project overview
- Creating the structure
- Moving the files
- Cleaning up

#### 06. Wildcards and Patterns
- The star wildcard
- The question mark wildcard
- Bracket wildcards
- Combining wildcards
- Recap: selective operations

#### 07. File Content
- Head and tail
- Word count
- Sort command
- Unique command
- Grep basics
- Grep with flags
- Recap: text detective

#### 08. Redirection
- Standard output
- Overwrite to a file
- Append to a file
- Standard input
- Standard error
- Recap: log builder

#### 09. Piping
- What is a pipe
- Chaining two commands
- Chaining multiple commands
- Pipe with grep
- Recap: data pipeline

#### 10. Log Analyzer Project
- Project overview
- Viewing the log file
- Filtering errors
- Counting occurrences
- Generating a report

#### 11. Permissions
- Understanding permissions
- Reading permissions
- chmod with numbers
- chmod with symbols
- File ownership
- Recap: lock it down

#### 12. Environment
- Environment variables
- View all variables
- The PATH variable
- Setting a variable
- Export a variable
- The profile file
- Recap: custom environment

#### 13. Shell Scripting Basics
- What is a shell script
- Create and run a script
- Variables in scripts
- User input in scripts
- If statements
- For loops
- While loops
- Recap: number guesser

#### 14. Backup Script Project
- Project overview
- Getting the source path
- Creating backup folder
- Copying the files
- Adding a timestamp
- Success message

#### 15. Final Challenges
- File organizer by type
- Word frequency counter
- System health checker

---

## Step 2: The Seven Layers of Networking

### Purpose
Explore how networks really function, one layer at a time. This section introduces the seven OSI layers and the related TCP/IP model, explaining what lives at each layer and how data is encapsulated as it traverses the network.

### Prerequisites
- Comfortable running commands in a terminal
- No networking knowledge required

### Key topics
- Layered architecture and communication models
- Physical and data link layers
- MAC addresses and ARP
- Network layer and routing fundamentals
- TCP, UDP, ports, and sockets
- Encapsulation and packet journey

### Syllabus

#### 01. Why Layers
- The layering idea
- Seven layers
- Numbers and names
- Recap: the stack

#### 02. The Physical Layer
- Bits on a wire
- How long will it take
- Duplex and collisions

#### 03. Data Link and MACs
- Frames and MACs
- Reading a MAC
- ARP
- Recap: layer two

#### 04. The Network Layer
- IP and TTL
- Ping and ICMP
- Routing tables
- Recap: layer three

#### 05. Transport: TCP and UDP
- Ports
- The TCP handshake
- TCP vs UDP
- Sockets in action
- Recap: layer four

#### 06. The Upper Layers
- Session and presentation
- The application layer
- Watching the layers
- Recap: top of the stack

#### 07. Encapsulation
- Wrapping data
- Headers everywhere
- Reading a capture
- Recap: follow the wrap

#### 08. The Journey of a Packet
- A click happens
- Crossing the networks
- Arriving and climbing
- Recap: end to end

---

## Step 3: IP Addressing and Subnetting

### Purpose
Master IP addresses from the bits up. This step builds the arithmetic and reasoning required to correctly design, divide, and troubleshoot network ranges.

### Prerequisites
- Comfortable running commands in a terminal
- No networking knowledge required

### Key topics
- Binary and decimal conversion
- IPv4 anatomy
- Subnet masks and CIDR
- Network and broadcast addresses
- Subnetting drills and VLSM
- Private address ranges and IPv6 basics

### Syllabus

#### 01. Bits and Binary
- Numbers as bits
- Binary to decimal
- Powers of two
- Recap: binary warm-up

#### 02. IPv4 Anatomy
- Four octets
- The 32-bit view
- Network and host
- Valid or not
- Recap: read an address

#### 03. The Subnet Mask
- What a mask does
- Mask to prefix
- Prefix to mask
- ANDing it out
- Recap: mask basics

#### 04. CIDR Notation
- Goodbye classes
- Reading CIDR
- Block sizes
- Recap: CIDR reading

#### 05. Inside a Subnet
- Network address
- Broadcast address
- Usable hosts
- First and last host
- Recap: subnet facts

#### 06. Subnetting Drills
- The block size trick
- Which subnet am I in
- Splitting a /24
- Splitting again
- Recap: subnetting drills

#### 07. Special Addresses
- Private ranges
- Loopback
- Link-local
- Zero and broadcast

#### 08. Planning Networks
- Right-sizing
- VLSM step by step
- Allocation order
- Checking for overlap
- Recap: plan a network

#### 09. IPv6 First Steps
- Why IPv6
- Reading IPv6
- Prefixes in IPv6
- Recap: IPv6 basics

#### 10. Capstone: The Office
- The floor plan
- Sizing each team
- Carving the block
- The router's view
- Recap: ship the plan

---

## Step 4: Hosts, DNS, Ports, and HTTP

### Purpose
Learn how machines actually talk to each other from the command line. This stage combines addressing, naming, reachability checks, and application-layer debugging using real hosts.

### Prerequisites
- Comfortable running commands in a terminal
- No networking knowledge required

### Key topics
- Clients and servers
- DNS and hostnames
- Ports and sockets
- `ping` and reachability checks
- HTTP request and response basics
- `curl` usage and debugging
- Sending data and HTTPS/TLS

### Syllabus

#### 01. How Networks Talk
- What is a network
- Clients and servers
- IP addresses
- Your first request

#### 02. Names and DNS
- Why names exist
- The hosts file
- Looking up a name
- Record types
- Recap: names

#### 03. Ports and Sockets
- What is a port
- Well-known ports
- Localhost
- Listening vs connecting
- Recap: ports

#### 04. Is It Reachable?
- ICMP and ping
- Reading ping output
- When ping lies
- Refused or timed out
- Recap: reachability

#### 05. HTTP by Hand
- Anatomy of a request
- Request methods
- Status codes
- Response headers
- Writing a response
- Recap: HTTP basics

#### 06. curl in Depth
- Seeing the headers
- Saving responses
- Custom headers
- Status and timing
- curl exit codes
- Recap: curl flags

#### 07. Sending Data
- POST with `-d`
- What `-d` actually sends
- Form encoding
- Sending JSON
- PUT and DELETE
- Recap: writing data

#### 08. HTTPS and TLS
- Why HTTPS
- The scheme picks the port
- Certificates and trust
- When TLS fails
- Recap: HTTPS

#### 09. Auth and Debugging
- API keys and authentication
- The user agent
- Timeouts and retries
- Reading a failure
- Recap: debugging

---

## Step 5: LAN Lab and Troubleshooting

### Purpose
Build, address, and troubleshoot small networks in a terminal lab. Learners move from theory to practical diagnostics by configuring hosts, switches, routers, gateways, and routing tables.

### Prerequisites
- Comfortable running commands in a terminal
- Basic understanding of IP addresses and subnet masks

### Key topics
- Creating a lab environment
- Addressing hosts on a LAN
- ARP and switch behavior
- Gateway and routing concepts
- Common network faults and fixes
- Scaling office networks and branch troubleshooting

### Syllabus

#### 01. Meet the Lab
- Your first lab
- Cables
- Wires are not enough
- Recap: build a lab

#### 02. Addresses on the LAN
- Giving a host an address
- The first ping
- What ARP learned
- The switch learns too
- Recap: first conversation

#### 03. Masks Matter
- Wrong mask, no reply
- Same switch, other subnet
- Let’s check: do the looking
- Recap: diagnose the pair

#### 04. Routers and Gateways
- Two networks
- The default gateway
- Both sides need a way back
- Routing tables
- Recap: connect the floors

#### 05. Troubleshooting I
- The unplugged host
- The missing gateway
- The typo in the mask
- Recap: two faults

#### 06. Troubleshooting II
- Two devices, one address
- The network address trap
- Gateway on the wrong subnet
- Recap: the silent router

#### 07. Growing the Office
- The plan
- Sales comes online
- Engineering joins
- Gateways everywhere
- Recap: the printer

#### 08. Capstone: Fix the Branch
- Survey the damage
- Fix the address clash
- Masks and gateways
- Recap: prove it works

---

## Step 6: Docker and Container Networking

### Purpose
Learn Docker from the command line and understand how containers get addresses, communicate with each other, and reach the outside world.

### Prerequisites
- Completion of the Fundamentals section

### Key topics
- Docker concepts and container architecture
- Pulling and running images
- Building custom Dockerfiles
- Copying files into and out of containers
- Volumes and networks
- Docker app project and final challenges

### Syllabus

#### 01. Introduction
- What is Docker
- Images and containers

#### 02. Images
- Pulling images
- Listing images
- Tagging images
- Recap: image wrangler

#### 03. Running Containers
- Running a command
- Real execution
- Different distros
- Auto-remove containers
- Recap: container runner

#### 04. Managing Containers
- Naming containers
- Detached containers
- Listing all containers
- Stopping and removing containers
- Recap: container lifecycle

#### 05. Inspecting Containers
- Container logs
- Inspecting details
- Executing commands
- Recap: health check

#### 06. Building Images
- What is a Dockerfile
- Building an image
- The CMD instruction
- Recap: image builder

#### 07. Files in Images
- The COPY instruction
- The WORKDIR instruction
- Recap: package it up

#### 08. Copying Files
- Copy into a container
- Copy out of a container

#### 09. Volumes and Networks
- Volumes
- Networks
- Recap: managing resources

#### 10. Greeting App Project
- Project overview
- Write the Dockerfile
- Build and run
- Personalize the greeting
- Ship a tagged version

#### 11. Final Challenges
- Distro detective
- Full build pipeline
- Runtime file drop

---

## Recommended Study Approach

For best results, work through the path in order, treating each step as a building block:

1. Build terminal confidence in Step 1.
2. Learn the abstraction model in Step 2.
3. Master the address math in Step 3.
4. Practice debugging with real protocols in Step 4.
5. Apply your knowledge in a simulated network environment in Step 5.
6. Expand into modern infrastructure concepts through Docker networking in Step 6.

This sequence mirrors how networking skills are commonly developed in professional IT, systems, and cloud environments.

## Final Note

This curriculum combines conceptual understanding with practical lab work. The value comes not from reading alone, but from using the commands, exploring packet flows, and repeatedly troubleshooting the kinds of issues that appear in real networks.
