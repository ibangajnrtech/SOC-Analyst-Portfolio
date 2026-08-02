# Domain Enrichment

Every time a user clicks a link or a system resolves a hostname, the Domain Name System (DNS) springs into action. DNS is the mechanism that converts human-friendly names like tryhackme.com into IP addresses that machines understand. 

**A / AAAA Records**

These records map the domain to IPv4 and IPv6 addresses. For example, tryhackme.com resolves to an Amazon IP address, which means it is either hosted in AWS cloud, or uses a proxy that uses AWS (in this case, Vercel CDN). You can then investigate the resolved IP and see if it is tied to a known APT infrastructure or maps to a known attack campaign.

**TXT Records**

TXT records can tell a lot about the domain: from its mail security settings (SPF/DKIM) to the tools it uses. An attack on infrastructure will generally have empty TXT records or point you to some interesting discoveries, such as suspicious SPF records or a faked DKIM signature, often abused during phishing campaigns.

## Attack Techniques Using DNS

- **CDN Abuse**
Attackers route malicious traffic through legitimate CDNs like Cloudflare, Akamai, or Fastly to hide the real origin server. If an A record points to a CDN range, the IP itself won't tell you much, since the same IP can front thousands of legitimate and malicious services simultaneously.
- **Typosquatting**
Domains like tryhakme[.]com or micros0ft[.]net rely on visual similarity to trick users. Treat any look-alike of a known brand as high risk. Note that companies can register obvious typos of their own domain defensively, so a typosquat resolving to the legitimate brand is not necessarily malicious.
- **IDN Attacks**
Domain names allow non-ASCII characters, which adversaries abuse by substituting Cyrillic or Greek letters that look identical to Latin ones. For example, tryhаckme[.]com uses a Cyrillic "а" in place of the Latin one. The domain looks the same, but resolves entirely differently. When in doubt, convert the domain to its Punycode representation (the xn-- form) using a tool like punycoder.com. If the result starts with xn--, the domain contains non-ASCII characters and needs a closer look.

# IP Enrichment

Query IP addresses in two services:
- **AbuseIPDB**: To see if the IP was involved in any port scans or brute-force attacks
- **VirusTotal**: To get the overall IP reputation and read its community comments (if any)

**Note**: if the IP is not within the CDN range, even 1 detection is an indicator of a risky IP

## Autonomous Systems

An Autonomous System (AS) is a collection of IP prefixes under a single organization's control. Each AS is assigned a unique number (ASN). BGP.Tools website would give you the most comprehensive info about the AS and its purpose. Looking at the ASN helps analysts assess the likely role of an IP, for example:

- **Residential ASNs**: Alerts on these may indicate VPN usage or compromised consumer devices.
Example: AS124888 (Vodafone)
- **Server Hostings**: The most risky role, as it is often used by adversaries to distribute malware.
Example: AS215439 (PLAY2GO)
- **Cloud/CDN ASNs**: Used by both legitimate services and adversaries, needs deeper analysis.
Example: AS16509 (Amazon AWS)

## Geolocation (GeoIP)

Geolocation enrichment complements ASN lookup and is useful during the investigation of anomalous logons. Tools like ipinfo.io and iplocation.net provide approximate country, but city-level accuracy is generally unreliable. Overall, geolocation lookup helps you with:

- **User logon analysis**: If a US-based employee logs on from the Netherlands, it might indicate an intrusion
- **Network analysis**: If you monitor a local European company, any traffic to, let's say, Vietnam is suspicious

# Service Exposure

Knowing what services are exposed on an IP is useful from two angles. The first is when you're looking at a victim's public IP address and trying to figure out how the attacker got in. If SSH is exposed, it's likely a starting point. The second is when you look up the attacker's IP. If it runs outdated services or has RDP exposed, the host was likely compromised and is being used as a jump point by attackers.

## Shodan Reconnaissance

Shodan is a powerful reconnaissance tool for IP address analysis. By indexing internet-connected devices and services, Shodan provides detailed information about open ports, running services, and system configurations.

## Censys Search

Censys.io can be a good alternative to Shodan for blue teams, as it shows exposed services even on non-standard ports and provides some advanced search capabilities.

## TLS Certificates

IPs that expose HTTPS services bring in a new interesting indicator: TLS certificates. We can use tools such as crt.sh to look at TLS certificate information as a gold mine for enrichment (Note that the service is often unavailable due to high request volume). Alternatively, we can investigate the certificates with Censys or SSL Shopper. The key fields to look out for include:

- **Issuer**: A self-signed certificate is a strong indicator that the website is worth investigating
- **Validity**: Newly-created and long-lived certificates can also indicate malware infrastructure
- **Subject**: For self-certificates, this can point to a program behind the HTTPS (e.g., Pfsense)

# VPN Detection

The SOC's job is to answer one question: Is this a real user, or someone behind a VPN?

## IP2Proxy and Spur

IP2Proxy and Spur are two essential resources for labeling VPN, proxy, and Tor exit nodes.

## SOC Analyst Workflow

- **Investigate the domain**
    - Does it "look" legit? Does it remind you of typosquatting?
    - What is its reputation? Is it a known malicious domain?
    - When was it registered? If it's new, that's suspicious.
    - Resolve the domain to IP and investigate the IP.
- **Investigate the IP**
    - Is it a CDN range? If yes, no need to investigate further.
    - What is its reputation? Use VirusTotal, AbuseIPDB, etc.
    - Where is it located? Is it a VPN node? How does it match the alert? (Example: US-based user is not expected to log in from Asia)
    - What AS type does it belong to? How does it match the alert? (Example: employees are not expected to log in from cloud hosting)