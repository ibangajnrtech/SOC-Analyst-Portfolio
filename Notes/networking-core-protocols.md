# DNS: Remembering Addresses

DNS is responsible for properly mapping a domain name to an IP address.
DNS operates at the Application Layer, i.e., Layer 7 of the ISO OSI model.
DNS traffic uses UDP port 53 by default and TCP port 53 as a default fallback.

## Types of DNS records

- **A record**: The A (Address) record maps a hostname to one or more IPv4 addresses. For example, you can set example.com to resolve to 172.17.2.172.
- **AAAA Record**: The AAAA record is similar to the A Record, but it is for IPv6.
- **CNAME Record**: The CNAME (Canonical Name) record maps a domain name to another domain name. For example, www.example.com can be mapped to example.com or even to example.org.
- **MX Record**: The MX (Mail Exchange) record specifies the mail server responsible for handling emails for a domain.

# Protocols, their Transport Protocols and their Port Numbers

1) Telnet -> TCP -> 23
2) DNS -> UDP or TCP -> 53
3) HTTP -> TCP -> 80
4) HTTPS -> TCP 443
5) FTP -> TCP -> 21
6) SMTP -> TCP -> 25
7) POP3 -> TCP -> 110
8) IMAP -> TCP -> 143
