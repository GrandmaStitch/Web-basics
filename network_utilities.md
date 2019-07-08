### Some useful network utility programs

- nmap, tcpdump, traceroute, mtr, net-tools

- Networking diagnostic tools including *ping*, *traceroute*, and *mtr* use Internet Control Message Protocol (ICMP) packets to test contention and traffic between two points on the Internet.


### nmap toolkit

- `nmap -sS -T4 <url> or <ipaddress>`
  scan all the opening ports

- Ncat is a general-purpose command-line tool for reading, writing, redirecting, and encrypting data across a network.
  ```
  ncat -C www.google.com 80
  GET / HTTP/1.0
  ```
  The -C option turns on CRLF replacement, which replaces any line endings you type with CRLF. CRLF line endings are required by many protocols, including HTTP, though many servers will accept a plain newline (LF) character.

  `ncat -l <url> <port>`
  listen to the port

  `ncat -l <url> <port> < hello.html`

  `ncat <url> <port>`

### Some useful commands

- `ip route show`

- `ping -c3 8.8.8.8`

- look up the IP address for a website
  `host <domain-name>`
  `dig <domain-name>`

- `host -t aaaa www.google.com`

- `host -t mx www.google.com`

- `tcpdump -c5 host www.google.com`

- `tcpdump -n host 8.8.8.8`

- `tcupdump -n port 53` (monitor the DNS lookup)

- `traceroute www.google.com`

- `mtr www.google.com`

- `printf 'HEAD / HTTP/1.1\r\nHost: www.google.com\r\n\r\n' | nc www.google.com 80`

- `printf 'GET / HTTP/1.1\r\nHost: www.google.com\r\n\r\n' | nc www.google.com 80`

- `ip addr show`

- `ip route show default`

- `netstat -nr`



