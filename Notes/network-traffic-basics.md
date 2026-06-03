# What is the purpose of Network Traffic Analysis (NTA)
Generally, we would use NTA to:
- Monitor Network Performance
- Check for abnormalities in the network
- Inspect the content of suspicious communication internally and externally

From a SOC perspective, NTA helps:
- Detecting suspicious or malicious activity
- Reconstructing attacks during incident response
- Verifying and validating alerts

# What Network Traffic can we observe?

1) **Application**

On the application layer, we can find two important information structures: the application header information and the application data itself (payload). This information will change depending on which application layer protocol is used. 

2) **Transport**

The application data and header are segmented and encapsulated at the transport layer into smaller pieces. Each piece includes a transport header, in most cases TCP or UDP.

3) **Internet**

When the transport layer sends down a segment, the internet layer also adds its header. If the segment is larger than the Maximum Transmission Unit (MTU), it will be divided into fragments, and a header will be added to each of them. The fields that are most often logged are the source and destination IP and TTL. This is sufficient for most use cases. But, if we want to, for example, detect fragmentation attacks, we will need to inspect the fragment offset and total length fields as well. There are different variations of a fragmentation attack. For example, an attacker can create tiny fragments to evade the IDS or mess up the reassembly of fragments by using overlapping byte ranges.

4) **Link**

Once the internet layer finishes encapsulation, the IP packet is sent to the link layer. The link layer adds its header as well, containing more addressing information. Most logs will display the source and destination MAC addresses. For certain types of attacks, for example, ARP poisoning or spoofing, the information in the logs won't be sufficient. For these types of attacks, we need the full packet and context. 

# Network Traffic Sources and Flows

A corporate network typically has some predetermined network flows and sources. Sources can be grouped into two categories namely:
- Intermediary
- Endpoint

Flows can be grouped into two categories namely:
- **North-South**: Traffic that exits or enters the LAN and passes the firewall.
- **East-West**: Traffic that stays within the LAN (including LAN that extends to the cloud).

## Sources

- **Intermediary Sources**: These are devices through which traffic mostly passes. While they generate some traffic, it is significantly lower than what endpoint devices generate. Under this category, we can find firewalls, switches, web proxies, IDS, IPS, routers, access points, wireless LAN controllers, and many more.
The traffic that originates from these devices comes from services like routing protocols (EIGRP, OSPF, BGP), management protocols (SNMP, PING), logging protocols (SYSLOG), and other supporting protocols (ARP, STP, DHCP).

- **Endpoint Sources**: These are devices where traffic originates and ends. Endpoint devices take the bulk of the network bandwidth. Devices that fall under this category are servers, hosts, IoT devices, printers, lab machines, cloud resources, mobile phones, tablets, and many more.

## Flows

A network traffic flow is typically determined by the services available in the network, such as Active Directory, SMB, HTTPS, and so on.

1) **North-South Traffic**: NS traffic is often monitored closely as it flows from the LAN to the WAN and vice versa. The most well-known services in this category are client-server protocols like HTTPS, DNS, SSH, VPN, SMTP, RDP, and many more. Each of these protocols has two streams: ingress (inbound) and egress (outbound). All of this traffic passes the firewall in one way or another. Configuring firewall rules and logging properly are key to visibility.

2) **East-West Traffic**: EW traffic stays within the corporate LAN, so it is often monitored less. However, it is important to keep track of these flows. When the network is compromised, an attacker will often exploit different services internally to move laterally within the network. There are many services within this category namely:
- **Directory, Authentication & Identity Services**
    - Kerberos / LDAP: Authentication/queries to Active Directory
    - RADIUS / TACACS+: Network access control
    - Certificate Authority issuing internal certifications
- **File shares & print services**
    - SMB/CIFS: Accessing network drives
    - IPP/LPD: Printing over the network
- **Router, switching, and infrastructure services**
    - DHCP traffic between hosts and the DHCP server
    - ARP broadcast messages
    - Internal DNS
    - Routing protocol messages
- **Application Communication**
    - Database Connections: SQL over TCP
    - Microservices APIs: REST or gRPC calls between services
- **Backup & Replication**
    - File Replication: Between data centers or to backup servers
    - Database Replication: MySQL binlog replication, PostgreSQL streaming, and more
- **Monitoring & Management**
    - SNMP: Device health metrics
    - Syslog: Centralized logging
    - NetFlow/IPFIX: Traffic flow telemetry
    - Other endpoint logs sent to a central logging server

## Flow Examples

- **HTTPS**: A host requests a website; this request is sent to the NGFW, which includes a web proxy. The web proxy will act as the web server and simultaneously establish a new TCP session with the actual web server and forward the clients' requests. When the web proxy receives the answer from the web server, it inspects its contents and then forwards it to the host if deemed safe. To summarize, we have two sessions, one between the client and the proxy and the other between the proxy and the web server. From the client's point of view, it has established a session with the web server.

- **External DNS**: DNS traffic within a corporate network starts when a host sends a DNS query. The host sends the query to the internal DNS server on port 53, which will then act on behalf of the host. First, it will check if it has an answer to the query in its cache; if not, it will send the query via the router, through the firewall, to the configured DNS servers. The answer will then follow the same path to the internal DNS server, which will then forward it to the host.

- **SMB with Kerberos**: When a host opens a share to, for example, \\FILESERVER\MARKETING, an SMB session is set up. First, authentication is done via Kerberos. When a user logged in on the host, it authenticated with the Key Distribution Center on the Domain Controller and received a Ticket Granting Ticket to request "service authentication tickets". Now, the host requests a service ticket using the Ticket Granting Ticket it received earlier. The host then uses this ticket to establish the SMB connection. Once the SMB session is set up, the host can access the share.


