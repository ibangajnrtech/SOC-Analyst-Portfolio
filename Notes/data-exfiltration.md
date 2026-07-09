# Data Exfiltration

Data exfiltration is the unauthorized transfer of sensitive data from a computer or other device to an external destination controlled by an adversary. It can be deliberate (insider) or via malware / compromised credentials. It's a primary objective for attackers who have breached a network. As a SOC analyst, our job is to detect and stop this before sensitive information walks out the door.

## Why Adversaries Perform Data Exfiltration

Data exfiltration is the act of stealing sensitive information from a network. Adversaries do this for several reasons:

- **Financial Gain:** Stolen data (e.g., credit card info, personal records) can be sold on the dark web or used for fraud.
- **Espionage:** Nation-state actors target intellectual property, trade secrets, or classified data for strategic advantage.
- **Ransomware & Extortion:** Attackers steal data and threaten to leak it unless a ransom is paid.
- **Disruption & Sabotage:** Some adversaries aim to damage reputations or operations by leaking internal data.
- **Persistence & Reconnaissance:** Exfiltrated data helps attackers understand the environment for future attacks.

## Common phases related to exfiltration

- **Discovery / Collection:** attacker locates sensitive files. 
- **Staging / Compression:** attacker aggregates, compresses, encrypts, or encodes files (ZIP, RAR, 7z, tar, base64, steganography).
- **Exfiltration transport:** transfer over network, removable media, cloud, or covert channels.
- **Command & Control (C2) coordination:** orchestrates transfer and confirms receipt.

## Techniques and Indicators

Detecting exfiltration requires correlating host and network-level indicators such as unusually large or frequent outbound uploads (proxy/firewall), long or high-entropy DNS queries, suspicious process command-lines and network connections (Sysmon/EDR), cloud storage API activity, removable-media events, and effective SOC L1 triage focuses on source host/user, destination, transferred volume, process identity/command-line, and supporting evidence across proxy, DNS, flow, host, and cloud logs.
1) **Network-based**: 
    Examples:
    - HTTP/HTTPS uploads to S3/Azure Blob/webmail
    - FTP/SFTP/SCP
    - DNS tunnelling
    - ICMP/covert protocols
    - custom TCP/UDP
    Indicator of Attack & where to look:
    - Proxy/web gateway logs (large POSTs, uploads to cloud endpoints)
    - firewall/NGFW flows (high bytes to single IP/ASN)
    - netflow (spikes/outbound flows)
    - DNS logs (long hostnames, TXT queries)
2) **Host-based**:
    Examples:
    - Powershell/Invoke-WebRequest
    - rclone
    - awscli
    - curl/wget
    - archive creation (zip/rar)
    - use of removable USBs
    - ADS/hidden streams
    Indicator of Attack & where to look:
    - Sysmon/EDR (Process Create, Network Connect, File Create events)
    - Windows Security (4663/4656 object access)
    - auditd/shell history on Linux
    - removable-media events
3) **Cloud exfiltration**:
    Examples:
    - S3 PutObject / multipart upload
    - Azure Blob uploads
    - Google Cloud Storage objects
    - Insert, Drive/SharePoint external sharing
    Indicator of Attack & where to look:
    - CloudTrail / Azure Activity / GCP Audit
    - cloud storage access logs
    - unusual service-account or IP activity
4) **Covert & encoding**:
    Examples:
    - DNS tunnelling
    - base64 or chunked encoding
    - steganography into images/audio
    - splitting files into many small requests (low-and-slow)
    Indicator of Attack & where to look:
    - DNS logs
    - proxy logs with many small POSTs
    - correlation of intermittent uploads + suspicious process activity
5) **Insider & collaboration tools**:
    Examples:
    - Slack/Teams/Dropbox/Google Drive/Box uploads or sharing to external users; compromised employee accounts
    Indicator of Attack & where to look:
    - Audit logs (share events, file downloads)
    - mail logs
6) **General IoAs & triage signals**:
    Examples:
    - A large outbound volume to external IPs/domains
    - unknown destination domains
    - suspicious processes/command lines
    - many file read events followed by an outbound connection
    - multipart/streamed uploads
    Indicator of Attack & where to look:
    - Correlate: Proxy/Firewall/Netflow
    - DNS
    - Sysmon/EDR (EventID 1/3/11)
    - mail server logs

## Data exfiltration through DNS Tunneling

DNS exfiltration abuses the Domain Name System, a protocol normally allowed through networks, to smuggle bytes encoded inside DNS queries/responses so firewalls and web proxies don't notice. Because DNS is typically allowed and often unfiltered or forwarded to public resolvers, it's attractive for covert channels.

## DNS Tunneling
DNS (Domain Name System) translates human-friendly domain names (e.g., example.com) into IP addresses and provides other record types (A, AAAA, TXT, MX, CNAME, etc.). Key points:

- DNS queries are ubiquitous: almost every host performs DNS lookups.
- DNS is normally allowed through firewalls and gateways, making it an attractive covert channel.
- DNS uses UDP (mostly) on port 53 for queries and responses; TCP is used for zone transfers or large responses.

Why attackers use DNS for exfiltration:

- Always-on service: DNS lookups are routine and often allowed outbound.
- High cover: queries look like normal requests unless inspected closely.
- Flexible payload: data can be encoded into the subdomain labels or TXT responses.

## Indicators of attack

When analysing DNS traffic for possible indicators of data exfiltration, we should look for:

- Many DNS queries are sent to a single external domain, especially with very high counts compared to the baseline.
- Long subdomain labels or unusually long full query names (> 60–100 characters).
- High entropy or Base32/Base64-like patterns in the query name (lots of mixed case letters, digits, -, = signs for base64).
- Rare record types (TXT, NULL) or many large TXT responses.
- Unusual response behavior: frequent NXDOMAIN (if attacker uses exfil-by-query without answering), or TCP/large UDP fragments for DNS.
- Queries at regular intervals (beaconing behaviour).

## Detecting through Wireshark using filters

1) `dns`
2) `dns.flags.response == 0`
3) `dns and frame.len > 70`: Find long queries (suspicious subdomain lengths)
4) `dns && dns.qry.name contains <DOMAIN NAME>`

# Data exfiltration through FTP

FTP (File Transfer Protocol) is one of the oldest protocols for transferring files between a client and server over a TCP/IP network. Attackers use it to move large amounts of data off a network, sometimes via compromised credentials, misconfigured servers, or ephemeral accounts. Detection relies on a mix of packet inspection (FTP only), server logs, SSH session metadata, and network flow/size/pattern analysis.

## How adversaries use FTP for exfiltration

- Use legitimate FTP servers (public or misconfigured internal servers) to stage/transfer data.
- Use compromised credentials (service accounts, user creds).
- Use non-standard ports or tunneling to blend with other traffic.

## Indicators of attack

What to look for:

- USER and PASS commands (cleartext credentials).
- STOR (upload) and RETR (download) commands: repeated or large transfers.
- Large data connections to unusual external IPs, especially outside business hours.
- Data channel openings on ephemeral ports (PASV) paired with large payloads.

## Detecting through wireshark using filters

1) `ftp or ftp-data`: Isolate FTP control and data
2) `ftp.request.command == "USER" || ftp.request.command == "PASS"`: Look for Credential
3) `ftp contains "STOR"`: Look for Anomalies in Filenames or Credentials
4) `ftp contains "csv"`: Look for suspicious files by filtering on the file extensions
5) `ftp and frame.len > 90`: Identifying the payload with a large payload size

# Data exfiltration through HTTP

Data exfiltration via HTTP is when an attacker moves sensitive data out of a target network using HTTP as the transport. HTTP is commonly abused because it blends with normal web traffic, can traverse firewalls and proxies, and can be obfuscated (encoding, encryption, tunneling). 

## How adversaries use HTTP for data exfiltration

- **POST uploads to external servers:** Bulk data is sent to attacker-controlled hosts or cloud storage in POST request bodies.
- **GET requests with encoded data:** Attacker squeezes small chunks into query strings or path segments (useful for low-and-slow exfiltration).
- **Use of common services / CDN:** Exfiltration disguised as uploads to popular services or attacker-controlled subdomains under reputable domains.
- **Custom headers:** Data placed in headers (e.g., X-Data: <base64>) may bypass some string-based DLP.
- **Chunked transfer / multipart:** Large payloads split into multiple requests to avoid size thresholds.
- **HTTPS/TLS tunneling:** The encrypted channel hides the payload; detection requires TLS inspection, SNI analysis, or metadata-based detection.
- **Staging via cloud services:** The attacker uploads to Dropbox/GitHub/Gist and then fetches externally.

## Indicators of Attack (IoAs) 

- Unusually large HTTP POST requests to external/unexpected hosts.
- HTTP requests to domains with low reputation / rarely seen in baseline traffic.
- Frequent small requests (beaconing) to the same host, followed by large uploads.
- Chunked or multipart transfers where multiple requests compose a larger file.

## Analyzing logs using Splunk (Search Query)

1) `index="data_exfil" sourcetype="http_logs"`: to get the `http_logs`.
2) `index="data_exfil" sourcetype="http_logs" method=POST`: filter on the POST method
3) `index="data_exfil" sourcetype="http_logs" method=POST | stats count avg(bytes_sent) max(bytes_sent) min(bytes_sent) by domain | sort - count`: checking the average amount of bytes
4) `index="data_exfil" sourcetype="http_logs" method=POST bytes_sent > 600 | table _time src_ip uri domain dst_ip bytes_sent | sort - bytes_sent`: Filter out benign traffic and isolate POSTs with large payloads

## HTTP Network Traffic Analysis using Wireshark

1) `http`: Global filter for http traffic
2) `http.request.method == "POST"`: Filter on POST requests
3) `http.request.method == "POST" and frame.len > 500`: Filter on traffic with frame length more than 500

# Data exfiltration through ICMP

ICMP is a network-layer protocol used for diagnostics and control (e.g., ping, TTL exceeded). Because it is commonly allowed through firewalls and typically inspected less strictly than TCP/UDP, attackers sometimes abuse ICMP to tunnel and exfiltrate data. Malicious actors encode data into ICMP payloads (echo request/reply, timestamp, info) and send it to a remote listener under their control.

## How adversaries use ICMP for exfiltration

Common techniques:
- ICMP echo (type 8) / reply (type 0) tunneling: attackers place encoded (base64, hex) chunks of files inside ICMP payloads. The remote server collects and decodes them.
- Custom ICMP types/codes: using uncommon ICMP types or non-zero codes to avoid signature-based detections.
- Fragmentation and reassembly: large payloads are split across multiple packets.
- Encryption/obfuscation: Encrypting or encrypting payloads (base64 is common) to look like random data.

Indicators that something may be malicious:
- Persistent ICMP sessions to an external host not used for legitimate monitoring.
- Unusually large ICMP payloads or frequent ICMP with payload > typical ping size.
- ICMP payloads that contain high-entropy data or patterns consistent with base64/hex.
- Bursts of ICMP are immediately followed by no other legitimate application traffic from the same host.

## Indicators of attack in Wireshark

Look for the following in a pcap when inspecting in Wireshark:

- **ICMP packet volumes:** a single host sending many ICMP echo requests to an external IP.
- **Large frame.len or icmp.payload:** pings with payloads much larger than typical (e.g., > 64 bytes). 
- **ICMP type/code unusual values:** e.g., unusual use of timestamp(13/14) or custom codes.
- **Regular timing (periodicity):** evenly spaced ICMP packets carrying similar-sized payloads.
- **Fragments with reassembly:** multiple ICMP fragments from the same src/dst pair.

## ICMP Network Traffic Analysis using Wireshark

1) `icmp`: Global filterfor icmp traffic
2) `icmp.type == 8`: Filter to isolate ICMP echo request packets
3) `icmp.type == 8 and frame.len > 100`: Filter on ICMP requests and focusing on frame length over 100