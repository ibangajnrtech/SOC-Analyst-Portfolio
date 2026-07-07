# Nmap Scans

The most common types of Nmap scans to identify in wireshark:
- TCP connect scans
- SYN scans
- UDP scans

It is essential to know how Nmap scans work to spot scan activity on the network. However, it is impossible to understand the scan details without using the correct filters.

## TCP flags in a nutshell

1) Global search: `tcp` and `udp`
2) Only Syn flag: `tcp.flags == 2`
3) SYN flag is set. The rest of the bits are not important: `tcp.flags.syn == 1`
4) Only ACK flag: `tcp.flags == 16`
5) ACK flag is set. The rest of the bits are not important: `tcp.flags.ack == 1`
6) Only SYN, ACK flags: `tcp.flags == 4`
7) SYN and ACK are set. The rest of the bits are not important: `(tcp.flags.syn == 1) and (tcp.flags.ack == 1)`
8) Only RST flag: `tcp.flags == 4`
9) RST flag is set. The rest of the bits are not important: `tcp.flags.reset == 1`
10) Only RST, ACK flags: `tcp.flags == 20`
11) RST and ACK are set. The rest of the bits are not important: `(tcp.flags.reset == 1) and (tcp.flags.ack == 1)`
12) Only FIN flag: `tcp.flags == 1`
13) FIN flag is set. The rest of the bits are not important: `tcp.flags.fin == 1`

## ARP Poisoning/Spoofing (Man In The Middle Attack)

ARP protocol, or Address Resolution Protocol (ARP), is the technology responsible for allowing devices to identify themselves on a network. Address Resolution Protocol Poisoning (also known as ARP Spoofing or Man In The Middle (MITM) attack) is a type of attack that involves network jamming/manipulating by sending malicious ARP packets to the default gateway. The ultimate aim is to manipulate the"IP to MAC address table"and sniff the traffic of the target host.

### ARP analysis in a nutshell:

- Works on the local machine
- Enables the communication between MAC addresses
- Not a secure protocol
- Not a routable protocol
- It doesn't have an authentication function
- Common patterns are request & response, announcement and gratuitous packets.

**Wireshark Global search filter:** `arp`
**ARP Options for grabbing the low-hanging fruits:**
1) Opcode 1: ARP requests -> `arp.opcode == 1`
2) Opcode 2: ARP responses -> `arp.opcode == 2`
3) Hunt: Arp scanning -> `arp.dst.hw_mac==00:00:00:00:00:00`
4) Hunt: Possible ARP poisoning detection -> `arp.duplicate-address-detected or arp.duplicate-address-frame`
5) Hunt: Possible ARP flooding from detection -> `((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address)`

# Identifying Hosts: DHCP, NetBIOS and Kerberos

A security analyst should know how to identify the host on the network apart from IP to MAC address match. 
Protocols that can be used in Host and User Identification:
- Dynamic Host Configuration Protocol (DHCP) traffic
- NetBIOS (NBNS) traffic
- Kerberos traffic

## DHCP Analysis

Dynamic Host Configuration Protocol (DHCP) is the technology responsible for managing automatic IP address and required communication parameters assignment.

## DHCP investigation in a nutshell:

1) Global search: `dhcp` or `bootp`
2) **DHCP Request** packets contain the hostname information: `dhcp.option.dhcp == 3`
3) **DHCP ACK** packets represent the accepted requests: `dhcp.option.dhcp == 5`
4) **DHCP NAK** packets represent denied requests: `dhcp.option.dhcp == 6`
5) **DHCP Request** options for grabbing the low-hanging fruits: `dhcp.option.hostname contains "keyword"`
    - Option 12:Hostname.
    - Option 50:Requested IP address.
    - Option 51:Requested IP lease time.
    - Option 61:Client's MAC address.
6) **DHCP ACK** options for grabbing the low-hanging fruits:
    - Option 15:Domain name.
    - Option 51:Assigned IP lease time.
    As the message could be unique according to the case/situation, It is suggested to read the message instead of filtering it. Thus, the analyst could create a more reliable hypothesis/result by understanding the event circumstances.

## NetBIOS (NBNS) Analysis

Network Basic Input/Output System is the technology responsible for allowing applications on different hosts to communicate with each other.

## NBNS investigation in a nutshell:

1) Global search: `nbns`
2) **NBNS** options for grabbing the low-hanging fruits: `nbns.name contains "keyword"`
    - Queries:Query details.
    - Query details could contain "name, Time to live (TTL) and IP address details"

## Kerberos Analysis

Kerberos is the default authentication service for Microsoft Windows domains. It is responsible for authenticating service requests between two or more computers over the untrusted network. The ultimate aim is to prove identity securely.

## Kerberos in a nutshell:

1) Global search: `kerberos`
2) User account search: `kerberos.CNameString contains "keyword"`, `kerberos.CNameString and !(kerberos.CNameString contains "$")`
    - **CNameString**: The username.
    **Note:**  Some packets could provide hostname information in this field. To avoid this confusion, filter the "$"value. The values end with"$"are hostnames, and the ones without it are user names.
3) **Kerberos** options for grabbing the low-hanging fruits: `kerberos.pvno == 5`, `kerberos.realm contains ".org"`, `kerberos.SNameString == "krbtg"`
    - pvno:Protocol version.
    - realm:Domain name for the generated ticket.
    - sname:Service and domain name for the generated ticket.
    - addresses:Client IP address and NetBIOS name.
    **Note:** the "addresses" information is only available in request packets.

# Tunnelling Traffic: ICMP and DNS

Traffic tunnelling is (also known as "port forwarding" transferring the data/resources in a secure method to network segments and zones. It can be used for "internet to private networks" and "private networks to internet" flow/direction. There is an encapsulation process to hide the data, so the transferred data appear natural for the case, but it contains private data packets and transfers them to the final destination securely.
Tunnelling provides anonymity and traffic security. Therefore it is highly used by enterprise networks. However, as it gives a significant level of data encryption, attackers use tunnelling to bypass security perimeters using the standard and trusted protocols used in everyday traffic like ICMP and DNS. Therefore, for a security analyst, it is crucial to have the ability to spot ICMP and DNS anomalies.

## ICMP Analysis

Tunnelling provides anonymity and traffic security. Therefore it is highly used by enterprise networks. However, as it gives a significant level of data encryption, attackers use tunnelling to bypass security perimeters using the standard and trusted protocols used in everyday traffic like ICMP and DNS. Therefore, for a security analyst, it is crucial to have the ability to spot ICMP and DNS anomalies.

## ICMP analysis in a nutshell

Usually, ICMP tunnelling attacks are anomalies appearing/starting after a malware execution or vulnerability exploitation. As the ICMP packets can transfer an additional data payload, adversaries use this section to exfiltrate data and establish a C2 connection. It could be a TCP, HTTP or SSH data. As the ICMP protocols provide a great opportunity to carry extra data, it also has disadvantages. Most enterprise networks block custom packets or require administrator privileges to create custom ICMP packets.

1) Global search: `icmp`
2) **ICMP** options for grabbing the low-hanging fruits: `data.len > 64 and icmp`
    - Packet length.
    - ICMP destination addresses.
    - Encapsulated protocol signs in ICMP payload.

## DNS Analysis

Domain Name System (DNS) is designed to translate/convert IP domain addresses to IP addresses. It is also known as a phonebook of the internet. As it is the essential part of web services, it is commonly used and trusted, and therefore often ignored. Due to that, adversaries use it in data exfiltration and C2 activities.

## DNS analysis in a nutshell

Similar to ICMP tunnels, DNS attacks are anomalies appearing/starting after a malware execution or vulnerability exploitation. Adversary creates (or already has) a domain address and configures it as a C2 channel. The malware or the commands executed after exploitation sends DNS queries to the C2 server. However, these queries are longer than default DNS queries and crafted for subdomain addresses. Unfortunately, these subdomain addresses are not actual addresses; they are encoded commands like this: **encoded-commands.maliciousdomain.com**.
When this query is routed to the C2 server, the server sends the actual malicious commands to the host. As the DNS queries are a natural part of the networking activity, these packets have the chance of not being detected by network perimeters.
1) Global search: `dns`
2) **DNS** options for grabbing the low-hanging fruits: `dns contains "dnscat"`, `dns.qry.name.len > 15 and !mdns`
    - Query length.
    - Anomalous and non-regular names in DNS addresses.
    - Long DNS addresses with encoded subdomain addresses.
    - Known patterns like dnscat and dns2tcp.
    - Statistical analysis like the anomalous volume of DNS requests for a particular target.
    **!mdns:** Disable local link device queries.

# Cleartext Protocol Analysis: FTP

Investigating cleartext protocol traces sounds easy, but when the time comes to investigate a big network trace for incident analysis and response, the game changes. Proper analysis is more than following the stream and reading the cleartext data. For a security analyst, it is important to create statistics and key results from the investigation process. 

## FTP Analysis

File Transfer Protocol (FTP) is designed to transfer files with ease, so it focuses on simplicity rather than security. As a result of this, using this protocol in unsecured environments could create security issues like:
1) MITM attacks
2) Credential stealing and unauthorised access
3) Phishing
4) Malware planting
5) Data exfiltration

## FTP analysis in a nutshell

1) Global search: `ftp`
2) **FTP** options for grabbing the low-hanging fruits: `---`
    - x1x series: Information request responses.
    - x2x series: Connection messages.
    - x3x series: Authentication messages.
    **Note:** "200" means command successful.
3) **x1x** series options for grabbing the low-hanging fruits: `ftp.response.code == 211`
    - 211: System status.
    - 212: Directory status.
    - 213: File status
4) **x2x** series options for grabbing the low-hanging fruits: `ftp.response.code == 227`
    - 220: Service ready.
    - 227: Entering passive mode.
    - 228: Long passive mode.
    - 229: Extended passive mode.
5) **x3x** series options for grabbing the low-hanging fruits: `ftp.response.code == 230`
    - 230: User login.
    - 231: User logout.
    - 331: Valid username.
    - 430: Invalid username or password
    - 530: No login, invalid password.
6) **FTP** commands for grabbing the low-hanging fruits: `ftp.request.command == "USER"`, `ftp.request.command == "PASS"`, `ftp.request.arg == "password"`
    - USER: Username.
    - PASS: Password.
    - CWD: Current work directory.
    - LIST: List.
7) Advanced usages examples for grabbing low-hanging fruits: `ftp.response.code == 530`, `(ftp.response.code == 530) and (ftp.response.arg contains "username")`, `(ftp.request.command == "PASS" ) and (ftp.request.arg == "password")`
    - Bruteforce signal: List failed login attempts.
    - Bruteforce signal: List target username.
    - Password spray signal: List targets for a static password.

# Cleartext Protocol Analysis: HTTP

## HTTP Analysis

Hypertext Transfer Protocol (HTTP) is a cleartext-based, request-response and client-server protocol. It is the standard type of network activity to request/serve web pages, and by default, it is not blocked by any network perimeter. As a result of being unencrypted and the backbone of web traffic, HTTP is one of the must-to-know protocols in traffic analysis. Following attacks could be detected with the help of HTTP analysis:
- Phishing pages
- Web attacks
- Data exfiltration
- Command and control traffic (C2)

## HTTP analysis in a nutshell

1) Global search: `http`, `http2`
**Note:** HTTP2 is a revision of the HTTP protocol for better performance and security. It supports binary data transfer and request&response multiplexing.
2) **HTTP Request Methods** for grabbing the low-hanging fruits: `http.request.method == "GET"`, `http.request.method == "POST"`, `http.request`
    - GET
    - POST
    - Request: Listing all requests
3) **HTTP Response Status Codes** for grabbing the low-hanging fruits: `http.response.code == 200`, `http.response.code == 401`, `http.response.code == 403`, `http.response.code == 404`, `http.response.code == 405`, `http.response.code == 503`
    - 200 OK: Request successful.
    - 301 Moved Permanently: Resource is moved to a new URL/path (permanently).
    - 302 Moved Temporarily: Resource is moved to a new URL/path (temporarily).
    - 400 Bad Request: Server didn't understand the request.
    - 401 Unauthorised: URL needs authorisation (login, etc.).
    - 403 Forbidden: No access to the requested URL. 
    - 404 Not Found: Server can't find the requested URL.
    - 405 Method Not Allowed: Used method is not suitable or blocked.
    - 408 Request Timeout:  Request look longer than server wait time.
    - 500 Internal Server Error: Request not completed, unexpected error.
    - 503 Service Unavailable: Request not completed server or service is down.
4) **HTTP Parameters** for grabbing the low-hanging fruits: `http.user_agent contains "nmap"`, `http.request.uri contains "admin"`, `http.request.full_uri contains "admin"`
    - User agent: Browser and operating system identification to a web server application.
    - Request URI: Points the requested resource from the server.
    - Full *URI: Complete URI information.
    **URI:** Uniform Resource Identifier.
5) **HTTP Parameters** for grabbing the low-hanging fruits: `http.server contains "apache"`, `http.host contains "keyword"`, `http.host == "keyword"`, `http.connection == "Keep-Alive"`, `data-text-lines contains "keyword"`
    - Server: Server service name.
    - Host: Hostname of the server
    - Connection: Connection status.
    - Line-based text data: Cleartext data provided by the server.
    - HTML Form URL Encoded: Web form information.

## User-Agent Analysis in a nutshell

1) Global search: `http.user_agent`
2) Research outcomes for grabbing the low-hanging fruits: `(http.user_agent contains "sqlmap") or (http.user_agent contains "Nmap") or (http.user_agent contains "Wfuzz") or (http.user_agent contains "Nikto")`
    - Different user agent information from the same host in a short time notice.
    - Non-standard and custom user agent info.
    - Subtle spelling differences. ("Mozilla" is not the same as  "Mozlilla" or "Mozlila")
    - Audit tools info like Nmap, Nikto, Wfuzz and sqlmap in the user agent field.
    - Payload data in the user agent field.

## Log4j Analysis 
A proper investigation starts with prior research on threats and anomalies going to be hunted.

## Log4j Analysis in a nutshell

1) Research outcomes for grabbing the low-hanging fruits: `http.request.method == "POST"`, `(ip contains "jndi") or ( ip contains "Exploit")`, `(frame contains "jndi") or ( frame contains "Exploit")`, `(http.user_agent contains "$") or (http.user_agent contains "==")`
    - The attack starts with a "POST" request
    - There are known cleartext patterns: **"jndi:ldap"** and **"Exploit.class"**.

# Encrypted Protocol Analysis: Decrypting HTTPS

When investigating web traffic, analysts often run across encrypted traffic. This is caused by using the Hypertext Transfer Protocol Secure (HTTPS) protocol for enhanced security against spoofing, sniffing and intercepting attacks. HTTPS uses TLS protocol to encrypt communications, so it is impossible to decrypt the traffic and view the transferred data without having the encryption/decryption key pairs. As this protocol provides a good level of security for transmitting sensitive data, attackers and malicious websites also use HTTPS. Therefore, a security analyst should know how to use key files to decrypt encrypted traffic and investigate the traffic activity.

## Additional Information for HTTPS:

1) **HTTPS Parameters** for grabbing the low-hanging fruits: `http.request`, `tls`, `tls.handshake.type == 1`, `tls.handshake.type == 2`, `ssdp`
    - Request: Listing all requests
    - TLS: Global TLS search
    - TLS Client Request
    - TLS Server response
    - Local Simple Service Discovery Protocol (SSDP)
    **Note:** SSDP is a network protocol that provides advertisement and discovery of network services.

Similar to the TCP three-way handshake process, the TLS protocol has its handshake process. The first two steps contain "Client Hello" and "Server Hello" messages. The given filters show the initial hello packets in a capture file. These filters are helpful to spot which IP addresses are involved in the TLS handshake.
- **Client Hello:** `(http.request or tls.handshake.type == 1) and !(ssdp)` 
- **Server Hello:** `(http.request or tls.handshake.type == 2) and !(ssdp)`

An encryption key log file is a text file that contains unique key pairs to decrypt the encrypted traffic session. These key pairs are automatically created (per session) when a connection is established with an SSL/TLS-enabled webpage. As these processes are all accomplished in the browser, you need to configure your system and use a suitable browser (Chrome and Firefox support this) to save these values as a key log file. To do this, you will need to set up an environment variable and create the SSLKEYLOGFILE, and the browser will dump the keys to this file as you browse the web. SSL/TLS key pairs are created per session at the connection time, so it is important to dump the keys during the traffic capture. Otherwise, it is not possible to create/generate a suitable key log file to decrypt captured traffic. You can use the "right-click" menu or "Edit --> Preferences --> Protocols --> TLS" menu to add/remove key log files.

Note that the packet details and bytes pane provides the data in different formats for investigation. Decompressed header info and HTTP2 packet details are available after decrypting the traffic. Depending on the packet details, you can also have the following data formats:

- Frame
- Decrypted TLS
- Decompressed Header
- Reassembled TCP
- Reassembled SSL