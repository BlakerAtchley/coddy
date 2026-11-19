# Networking with the Terminal

**Coddy Computer Networking path, Step 4** · 9 chapters · 47 lessons · 47 graded challenges · Certificate earned 2026-11-10 ([verify](https://coddy.tech/certifications/tj6FfM-qnnXQ5))

[← Back to portfolio summary](README.md)

> **About the commands below:** Coddy's browser terminal grades each challenge live but does not keep a copy of what was typed. The commands here are reference solutions I wrote afterward for each challenge I passed. They are working commands that produce the graded output, with notes on why each flag is there. Many challenges start a throwaway server first. `sleep 1` gives it time to bind before the request.

## What this course covers

How machines talk, tested from the command line: ping and round trips, clients and servers, DNS and `/etc/hosts`, ports and sockets, reachability vs. service availability, raw HTTP over `nc`, `curl` in depth (headers, saving, timing, exit codes), sending form and JSON data, HTTPS/TLS failures, auth headers, user agents, timeouts, and a structured debugging ladder.

Two helpers recur throughout:

```bash
# A throwaway web server for the current folder
python3 -m http.server PORT > /dev/null 2>&1 &

# A "capture listener": answers 200 and saves the raw request to req.txt
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l PORT > req.txt &
```

## Contents

1. [How Networks Talk](#1-how-networks-talk)
2. [Names and DNS](#2-names-and-dns)
3. [Ports and Sockets](#3-ports-and-sockets)
4. [Is It Reachable?](#4-is-it-reachable)
5. [HTTP by Hand](#5-http-by-hand)
6. [curl in Depth](#6-curl-in-depth)
7. [Sending Data](#7-sending-data)
8. [HTTPS and TLS](#8-https-and-tls)
9. [Auth and Debugging](#9-auth-and-debugging)
10. [curl exit codes and flags reference](#curl-exit-codes-and-flags-reference)

---

## 1. How Networks Talk

### 1.1 What Is a Network · Easy
**Task:** Ping localhost once and print only how many packets came back.
```bash
ping -c1 127.0.0.1 | grep -o "1 packets received"
```
`grep -o` prints just the matching text rather than the whole summary line.

### 1.2 Clients and Servers · Easy
**Task:** Start a server on 9102 and confirm it's listening.
```bash
python3 -m http.server 9102 > /dev/null 2>&1 &
sleep 1
nc -z 127.0.0.1 9102 && echo listening || echo nothing
```

### 1.3 IP Addresses · Medium
**Task:** Pull the target address out of ping's first output line.
```bash
ping -c1 127.0.0.1 | head -1 | awk '{print $2}'
```
The first line is `PING 127.0.0.1 (127.0.0.1): 56 data bytes`, and `awk` takes the 2nd word.

### 1.4 Your First Request · Easy
**Task:** Serve `hello.txt` on 9101 and fetch it.
```bash
echo "hello from your first server" > hello.txt
python3 -m http.server 9101 > /dev/null 2>&1 &
sleep 1
curl -s http://127.0.0.1:9101/hello.txt
```

---

## 2. Names and DNS

### 2.1 Why Names Exist · Medium
**Task:** Fetch the same page by IP and by name.
```bash
echo SAME > index.html
python3 -m http.server 9103 > /dev/null 2>&1 &
sleep 1
curl -s http://127.0.0.1:9103/
curl -s http://localhost:9103/
```
Both requests reach the same server because `localhost` resolves to `127.0.0.1`.

### 2.2 The Hosts File · Medium
**Task:** Print the address of the first `/etc/hosts` line that mentions localhost.
```bash
grep localhost /etc/hosts | head -1 | awk '{print $1}'
```
`/etc/hosts` is checked before DNS, which makes it a common place for overrides (and for malware tampering).

### 2.3 Looking Up a Name · Medium
**Task:** Show a lookup failing on a reserved name.
```bash
getent hosts nope.invalid > /dev/null 2>&1 && echo resolved || echo failed
```
`.invalid` is reserved and will never resolve. `nslookup` or `host` would also work. The script tests the exit status.

### 2.4 Record Types · Hard
**Task:** Build a small zone file and extract the MX target.
```bash
echo "shop.example.com  A      93.184.216.34" > records.txt
echo "www.example.com   CNAME  shop.example.com" >> records.txt
echo "example.com       MX     mail.example.com" >> records.txt
echo "example.com       TXT    site-verification=abc123" >> records.txt
awk '$2 == "MX" {print $3}' records.txt
```
- **A** maps a name to an IPv4 address.
- **CNAME** makes a name an alias of another name.
- **MX** names the mail server for a domain.
- **TXT** holds free text, often used for domain-ownership verification (for example, adding a custom domain in Azure).

### 2.5 Recap: Names · Medium
**Task:** Do the reverse lookup: print the name mapped to `127.0.0.1`.
```bash
grep "^127.0.0.1" /etc/hosts | head -1 | awk '{print $2}'
```
The `^` anchor matches only lines that **start** with the address.

---

## 3. Ports and Sockets

### 3.1 What Is a Port · Hard
**Task:** Run two servers on one machine and show that the port picks the program.
```bash
mkdir a b
echo AAA > a/i.txt
echo BBB > b/i.txt
(cd a && python3 -m http.server 9104 > /dev/null 2>&1 &)
(cd b && python3 -m http.server 9105 > /dev/null 2>&1 &)
sleep 1
curl -s http://127.0.0.1:9104/i.txt
curl -s http://127.0.0.1:9105/i.txt
```
The subshell `( … )` starts each server in its own folder without changing the current directory.

### 3.2 Well-Known Ports · Hard
**Task:** Read the SSH and HTTPS port entries from `/etc/services`.
```bash
grep "^ssh\s" /etc/services | head -1 | awk '{print $2}'
grep "^https\s" /etc/services | head -1 | awk '{print $2}'
```
Output: `22/tcp` and `443/tcp`.

### 3.3 Localhost · Medium
**Task:** Serve a page on 9106 and count the response bytes.
```bash
echo "local traffic only" > page.html
python3 -m http.server 9106 > /dev/null 2>&1 &
sleep 1
curl -s http://localhost:9106/page.html | wc -c
```
Output: `19` (18 characters plus the newline).

### 3.4 Listening vs Connecting · Medium
**Task:** Check one open port and one closed port.
```bash
python3 -m http.server 9107 > /dev/null 2>&1 &
sleep 1
nc -z 127.0.0.1 9107 && echo open || echo closed
nc -z 127.0.0.1 9999 && echo open || echo closed
```

### 3.5 Recap: Ports · Medium
**Task:** Listen, check and connect, using the name `localhost`.
```bash
echo "ports are doors" > index.html
python3 -m http.server 9108 > /dev/null 2>&1 &
sleep 1
curl -s http://localhost:9108/index.html
```

---

## 4. Is It Reachable?

### 4.1 ICMP and ping · Easy
**Task:** Ping twice and print only the packet-loss figure.
```bash
ping -c2 127.0.0.1 | grep -o "0% packet loss"
```

### 4.2 Reading Ping Output · Hard
**Task:** Print only the number of packets received.
```bash
ping -c2 127.0.0.1 | grep "packets received" | awk -F', ' '{print $2}' | awk '{print $1}'
```
Split the summary on `, `, take the "2 packets received" field, then its first word.

### 4.3 When Ping Lies · Medium
**Task:** Show a host that's up but has nothing serving on the port.
```bash
ping -c1 127.0.0.1 > /dev/null 2>&1 && echo "host up"
nc -z 127.0.0.1 9999 && echo "port open" || echo "port closed"
```
A successful ping proves the host is reachable at L3. It proves nothing about whether a service is listening (L4/L7).

### 4.4 Refused or Timed Out · Hard
**Task:** Trigger both failure types and print their curl exit codes.
```bash
curl -s http://127.0.0.1:9999/ > /dev/null; echo "refused=$?"
curl -s --max-time 2 http://10.255.255.1/ > /dev/null; echo "timeout=$?"
```
- **7** means connection refused: the host answered with a TCP RST because nothing is listening.
- **28** means timed out: the packets were silently dropped, which usually points to a firewall or blackhole.

### 4.5 Recap: Reachability · Hard
**Task:** Climb the reachability ladder (host, port, page) against your own server.
```bash
echo up > index.html
python3 -m http.server 9109 > /dev/null 2>&1 &
sleep 1
ping -c1 127.0.0.1 > /dev/null 2>&1 && echo "1: ok"
nc -z 127.0.0.1 9109 && echo "2: ok"
echo "3: $(curl -s http://127.0.0.1:9109/index.html)"
```

---

## 5. HTTP by Hand

### 5.1 Anatomy of a Request · Hard
**Task:** Send a raw HTTP request with `nc` and print the status line.
```bash
echo raw > index.html
python3 -m http.server 9110 > /dev/null 2>&1 &
sleep 1
printf 'GET /index.html HTTP/1.0\r\n\r\n' | nc 127.0.0.1 9110 | head -1
```
Output: `HTTP/1.0 200 OK`. The blank line (`\r\n\r\n`) marks the end of the request headers.

### 5.2 Request Methods · Medium
**Task:** POST to a server that only supports GET and HEAD.
```bash
python3 -m http.server 9111 > /dev/null 2>&1 &
sleep 1
curl -s -X POST -o /dev/null -w "%{http_code}\n" http://127.0.0.1:9111/
```
Output: `501` (Not Implemented).

### 5.3 Status Codes · Medium
**Task:** Get a 200 and a 404 from the same server.
```bash
echo here > index.html
python3 -m http.server 9112 > /dev/null 2>&1 &
sleep 1
curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:9112/index.html
curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:9112/missing.html
```

### 5.4 Response Headers · Medium
**Task:** Fetch the headers only and print the content type.
```bash
echo hi > index.html
python3 -m http.server 9113 > /dev/null 2>&1 &
sleep 1
curl -sI http://127.0.0.1:9113/index.html | grep -i "^content-type"
```
`-I` sends a HEAD request, so you get the headers without the body.

### 5.5 Writing a Response · Expert
**Task:** Act as the server: hand-write an HTTP response with `nc -l`.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Type: text/plain\r\n\r\nhand written' | nc -l 9114 &
sleep 1
curl -s http://127.0.0.1:9114/
```

### 5.6 Recap: HTTP Basics · Expert
**Task:** Serve a hand-written 404, then print its status code and its body.
```bash
resp='HTTP/1.1 404 Not Found\r\nContent-Type: text/plain\r\n\r\nnothing here'
printf "$resp" | nc -l 9115 &
sleep 1
curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:9115/
printf "$resp" | nc -l 9115 &
sleep 1
curl -s http://127.0.0.1:9115/; echo
```
Each `nc -l` serves a single client and exits, so the second request needs a second listener.

---

## 6. curl in Depth

### 6.1 Seeing the Headers · Medium
**Task:** Include the response headers and print the status line.
```bash
echo hi > index.html
python3 -m http.server 9116 > /dev/null 2>&1 &
sleep 1
curl -si http://127.0.0.1:9116/index.html | head -1
```
`-i` includes the response headers in the output, which is different from `-I` (HEAD only).

### 6.2 Saving Responses · Medium
**Task:** Save the body to a file and count its bytes.
```bash
echo "saved to disk" > index.html
python3 -m http.server 9117 > /dev/null 2>&1 &
sleep 1
curl -s -o copy.html http://127.0.0.1:9117/index.html
wc -c < copy.html
```
Output: `14`.

### 6.3 Custom Headers · Expert
**Task:** Send `X-Course: networking` and prove the server received it.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9118 > req.txt &
sleep 1
curl -s --max-time 3 -H "X-Course: networking" http://127.0.0.1:9118/ > /dev/null
grep -i "^x-course" req.txt
```

### 6.4 Status and Timing · Hard
**Task:** Print the status code and the bytes received on one line.
```bash
echo hi > index.html
python3 -m http.server 9119 > /dev/null 2>&1 &
sleep 1
curl -s -o /dev/null -w "%{http_code} %{size_download}\n" http://127.0.0.1:9119/index.html
```
Output: `200 3`. `-w` can also print timing, for example `%{time_total}`.

### 6.5 curl Exit Codes · Hard
**Task:** Show that a 404 is still a successful request as far as curl is concerned.
```bash
python3 -m http.server 9120 > /dev/null 2>&1 &
sleep 1
code=$(curl -s -o /dev/null -w "%{http_code}" http://127.0.0.1:9120/nope.html); rc=$?
echo "status=$code"
echo "exit=$rc"
```
curl exits with 0 because the transfer itself worked. Add `--fail` (`-f`) to make HTTP errors produce a non-zero exit code in scripts.

### 6.6 Recap: curl Flags · Hard
**Task:** Combine four flags: silent, discard the body, 5-second timeout, and a custom output format.
```bash
echo done > index.html
python3 -m http.server 9121 > /dev/null 2>&1 &
sleep 1
curl -s -o /dev/null --max-time 5 -w "%{http_code} %{size_download}\n" http://127.0.0.1:9121/index.html
```
Output: `200 5`.

---

## 7. Sending Data

### 7.1 POST with -d · Expert
**Task:** POST `name=ada` to `/signup` and read back the request line the server received.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9122 > req.txt &
sleep 1
curl -s --max-time 3 -d "name=ada" http://127.0.0.1:9122/signup > /dev/null
head -1 req.txt
```
Output: `POST /signup HTTP/1.1`. Using `-d` switches curl to POST automatically.

### 7.2 What -d Actually Sends · Expert
**Task:** Send two `-d` fields and print the body the server received.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9123 > req.txt &
sleep 1
curl -s --max-time 3 -d "name=ada" -d "role=eng" http://127.0.0.1:9123/ > /dev/null
tail -1 req.txt; echo
```
curl joins multiple `-d` fields with `&`, giving `name=ada&role=eng`.

### 7.3 Form Encoding · Hard
**Task:** Confirm the Content-Type that curl picks for `-d`.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9124 > req.txt &
sleep 1
curl -s --max-time 3 -d "a=1" http://127.0.0.1:9124/ > /dev/null
grep -i "^content-type" req.txt
```
Output: `Content-Type: application/x-www-form-urlencoded`.

### 7.4 Sending JSON · Expert
**Task:** POST a JSON body to `/users` with the correct label.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9125 > req.txt &
sleep 1
curl -s --max-time 3 -H "Content-Type: application/json" -d '{"name":"ada"}' http://127.0.0.1:9125/users > /dev/null
grep -i "^content-type" req.txt
```

### 7.5 PUT and DELETE · Hard
**Task:** Send a DELETE and print the request line.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9126 > req.txt &
sleep 1
curl -s --max-time 3 -X DELETE http://127.0.0.1:9126/item/7 > /dev/null
head -1 req.txt
```

### 7.6 Recap: Writing Data
**Task:** PUT a JSON body to `/users/3`, then print the request line, the Content-Type and the body.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9127 > req.txt &
sleep 1
curl -s --max-time 3 -X PUT -H "Content-Type: application/json" -d '{"role":"admin"}' http://127.0.0.1:9127/users/3 > /dev/null
head -1 req.txt
grep -i "^content-type" req.txt
tail -1 req.txt; echo
```
This is the same request pattern used by REST APIs, including Azure Resource Manager.

---

## 8. HTTPS and TLS

### 8.1 Why HTTPS · Hard
**Task:** Show that a password sent over plain HTTP is readable on the wire.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9128 > req.txt &
sleep 1
curl -s --max-time 3 -d "password=hunter2" http://127.0.0.1:9128/login > /dev/null
tail -1 req.txt; echo
```
Anyone between the client and the server can read `password=hunter2`. TLS is what prevents that.

### 8.2 The Scheme Picks the Port · Hard
**Task:** Ask a plain-HTTP server for TLS.
```bash
python3 -m http.server 9129 > /dev/null 2>&1 &
sleep 1
curl -s https://127.0.0.1:9129/ > /dev/null; echo "tls=$?"
```
Output: `tls=35` (SSL connect error). The server answered, but it doesn't speak TLS.

### 8.3 Certificates and Trust · Medium
**Task:** Confirm the system CA bundle exists.
```bash
[ -f /etc/ssl/certs/ca-certificates.crt ] && echo "ca bundle present" || echo missing
```
Every TLS certificate is checked against this list of trusted root CAs.

### 8.4 When TLS Fails · Hard
**Task:** Request the same port over http:// and https://.
```bash
echo plain > index.html
python3 -m http.server 9130 > /dev/null 2>&1 &
sleep 1
curl -s http://127.0.0.1:9130/index.html > /dev/null; echo "http=$?"
curl -s https://127.0.0.1:9130/index.html > /dev/null; echo "https=$?"
```

### 8.5 Recap: HTTPS · Hard
**Task:** Check the CA bundle, then get the TLS error code from a plain server.
```bash
[ -f /etc/ssl/certs/ca-certificates.crt ] && echo "ca=yes"
python3 -m http.server 9131 > /dev/null 2>&1 &
sleep 1
curl -s https://127.0.0.1:9131/ > /dev/null; echo "tls=$?"
```

---

## 9. Auth and Debugging

### 9.1 API Keys and Bearer · Hard
**Task:** Send a bearer token and confirm it arrived.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9132 > req.txt &
sleep 1
curl -s --max-time 3 -H "Authorization: Bearer t0ken" http://127.0.0.1:9132/me > /dev/null
grep -i "^authorization" req.txt
```
Bearer tokens are how Azure (Entra ID / OAuth 2.0) APIs authenticate callers. Never send one over plain HTTP.

### 9.2 The User Agent · Hard
**Task:** Identify the client as `my-monitor/1.0`.
```bash
printf 'HTTP/1.1 200 OK\r\nContent-Length: 0\r\n\r\n' | nc -l 9133 > req.txt &
sleep 1
curl -s --max-time 3 -A "my-monitor/1.0" http://127.0.0.1:9133/ > /dev/null
grep -i "^user-agent" req.txt
```

### 9.3 Timeouts and Retries · Medium
**Task:** Put a time limit on a request that will never be answered.
```bash
curl -s --max-time 2 http://10.255.255.1/ > /dev/null; echo "gave up: $?"
```
Output: `gave up: 28`. Monitoring scripts should always set a timeout.

### 9.4 Reading a Failure · Expert
**Task:** Produce three failures and label each by its exit code.
```bash
curl -s http://127.0.0.1:9999/ > /dev/null; echo "refused: $?"
curl -s --max-time 2 http://10.255.255.1/ > /dev/null; echo "dropped: $?"
curl -s https://127.0.0.1:9999/ > /dev/null; echo "notls: $?"
```
The third returns `7`, not `35`. With nothing listening, TCP refuses the connection before TLS is ever attempted.

### 9.5 Recap: Debugging · Final
**Task:** Run the full troubleshooting ladder: name, port, status, body.
```bash
echo served > index.html
python3 -m http.server 9134 > /dev/null 2>&1 &
sleep 1
grep -q localhost /etc/hosts && echo "name: ok"
nc -z 127.0.0.1 9134 && echo "port: ok"
echo "status: $(curl -s -o /dev/null -w '%{http_code}' http://localhost:9134/index.html)"
echo "body: $(curl -s http://localhost:9134/index.html)"
```
This bottom-up ladder (resolve, connect, status, content) is the same order I'd use to troubleshoot an Azure App Service or a VM behind an NSG.

---

## curl exit codes and flags reference

| Exit code | Meaning | Typical cause |
| ---: | --- | --- |
| 0 | Transfer worked (even with a 404!) | Use `-f` to fail on HTTP errors |
| 7 | Couldn't connect | Nothing listening; RST returned |
| 28 | Timed out | Firewall or blackhole dropping packets |
| 35 | SSL connect error | Speaking TLS to a plain-HTTP port |

| Flag | Purpose |
| --- | --- |
| `-s` | Silent (no progress bar) |
| `-o FILE` / `-o /dev/null` | Save the body / discard it |
| `-i` / `-I` | Include headers / HEAD request only |
| `-w "%{http_code} %{size_download}"` | Print transfer facts |
| `-X METHOD` | PUT, DELETE, etc. |
| `-d DATA` | Send a body (switches to POST) |
| `-H "Name: value"` | Custom header |
| `-A AGENT` | User-Agent |
| `--max-time N` | Overall timeout |
