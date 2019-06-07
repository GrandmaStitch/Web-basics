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

`ncat -l <url> <port> <hello.html`

`ncat <url> <port>`

