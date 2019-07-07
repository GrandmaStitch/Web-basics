### Network Traffic

![network traffic](imgs/NetworkTraffic.png)

![network traffic](imgs/NetworkTraffic1.png)

![The IETF Model](imgs/IETFmodel.png)

&nbsp;

### Names and Addresses

- Hosts: a machine on the internet that might host services.

- Endpoints: the two machines or programs communicating over the connection.

- DNS(Domain Name System): the way that a hostname gets translated into an IP address.

  - The Resolver: the DNS client code built into every operating system. 

  - DNS record types:

    - A(A-Record): IPv4 address record. It is the IPv4 IP address belonging to the hostname of the domain. 
    - AAAA: IPv6 address record. 
    - ANY: all records of all types known to the name server
    - CNAME: Canonical name record. Alias of one name to another: the DNS lookup will continue by retrying the lookup with the new name.
    - MX: Mail exchange record. Maps a domain name to a list of message transfer agents for that domain.
    - NS: Name server record.
    - PTR: Pointer record.
    - SIG: Signature.
    - SOA: Start of authority record.

  - `www.google.com` is the CNAME of the `google.com`. It's pretty much a style and branding preference.


