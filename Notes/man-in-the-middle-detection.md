# Man In The Middle Overview

A Man-in-the-Middle (MITM) attack is a cyberattack where an attacker secretly intercepts and potentially alters communication between two parties, such as a user and a service, without their knowledge. The attacker may eavesdrop to steal sensitive data like credentials and credit card info or inject malicious content.

## How MITM Attacks Work

MITM attacks generally involve two main steps:

- Interception: The attacker inserts themselves into a communication stream, often by exploiting weaknesses in network protocols or by using techniques like ARP, DNS, or IP spoofing.
- Manipulation/Decryption: The attacker tries to access or modify the communication, decrypting encoded data or injecting harmful content, such as altered website responses or fake login forms.

## Common Types of MITM Attacks

- **Packet sniffing:** Capturing unencrypted data packets exchanged over a network, often on open Wi-Fi.
- **Session hijacking:** Stealing and using session tokens to impersonate users.
- **SSL stripping:** Downgrading HTTPS connections to insecure HTTP to steal or alter data transferred.
- **DNS spoofing:** Redirecting legitimate website traffic to fraudulent domains by manipulating DNS responses.
- **IP spoofing:** Crafting malicious IP packets that appear to come from trusted systems.
- **Rogue Wi-Fi access point:** Creating fake networks to intercept user traffic.

## Situating MITM within the Cyber Kill Chain

Man-in-the-Middle is a versatile technique that adversaries primarily leverage during the Exploitation and Installation phases.

- **As an Exploitation Technique:** A MITM attack exploits the inherent trust and design limitations of core network protocols like ARP and DNS. By manipulating these protocols, an attacker can intercept a communication channel. This act of session interception is a form of exploitation, as it violates the integrity of the network and provides the adversary with their initial foothold for eavesdropping or active manipulation.
- **As an Installation Vector:** Once an attacker has successfully established a MITM position, they control the data stream and can use it as a delivery mechanism for malicious payloads. For example, an attacker can inject a browser exploit, a malware dropper, or a remote access trojan (RAT) into legitimate, unencrypted downloads. This action corresponds to the Installation phase, where the adversary establishes persistence or deploys additional malicious tools onto the victim's system.

## Detecting ARP Spoofing - What Is the ARP Protocol

ARP (Address Resolution Protocol) maps IP addresses to MAC addresses in a local network. When a device wants to send data to another IP, it first asks: "Who has this IP?” The correct device replies with its MAC address.

## ARP Spoofing

In ARP spoofing, an attacker sends fake ARP replies to trick devices into associating the attacker’s MAC address with a legitimate IP, usually the default gateway. This allows the attacker to intercept, modify, or redirect traffic.

## Why ARP Spoofing Works

ARP has no authentication. Any device can send unsolicited is-at messages. An attacker exploits this vulnerability by sending fake ARP replies to victims and gateways:

- 192.16.10.100 is at 02:fe:BB:cd:55:55 attacker claims to be the gateway.

Results:

- The Victim's ARP cache becomes poisoned.
- All traffic intended for the gateway flows through the attacker first (MITM).

## Indicators of the Attack

We can look for the following key indicators while investigating the logs or network traffic for a potential Man-in-the-Middle attack using ARP spoofing.

- **Duplicate MAC-to-IP Mappings:** Multiple MAC addresses claiming the same IP address. Indicates impersonation.
- **Unsolicited ARP Replies:** High number of ARP replies without matching requests ("gratuitous ARP").
- **Abnormal ARP Traffic Volume:** A Large number of ARP packets in short intervals.
- **Unusual Traffic Routing:** Traffic rerouted through the attacker’s MAC.
- **Gateway Redirection Patterns:** Multiple destination MACs for the same gateway IP.
- **ARP Probe / Reply Loops:** Many ARP requests with Who has 192.168.1.x? Tell 192.168.1.y patterns.

## Narrowing down ARP traffic

1) `arp`: Global search filter for arp packets
2) `arp.opcode == 1`: Filter to look for all ARP requests captured from different hosts.
3) `arp.opcode == 2`: Filter to look for all ARP responses 
    **NOTE:** Legitimate replies typically correspond to recent "who-has" requests. Suspicious activity is indicated by numerous replies with no visible requests or repeated advertisements of the same IP address from a suspicious MAC address.
4) `arp.isgratuitous`: Filter to retrieve only gratuitous ARP packets.
    **NOTE:** A suspicious host sends many unsolicited (gratuitous) ARP replies, especially to multiple destinations. Repeated gratuitous ARPs can indicate an attacker maintaining their poison state.
5) `arp && arp.src.proto_ipv4 == LEGIT_GATEWAY_ROUTER_IP && eth.src == SUSPECTED_ATTACKER_MAC`: Filter to examine the ARP traffic associated with the gateway
6) `arp.opcode == 2 && arp.src.proto_ipv4 == LEGIT_GATEWAY_ROUTER_IP`: Filter to narrow down on the gateway IP to further probe about the MAC addresses associated with the IP.
7) `arp.duplicate-address-detected || arp.duplicate-address-frame`: Filter to check for duplicate MAC address mappings to a single IP address.

## Unmasking DNS Spoofing - DNS Protocol Simplified

DNS Spoofing (or DNS Cache Poisoning) is when an attacker corrupts this system. They give your computer the wrong "phone number" for a website you're trying to visit.

This is a powerful technique for launching a Man-in-the-Middle (MITM) attack. Here's how it works:

- The Victim tries to visit their bank at my-real-bank.com.
- The Attacker, who is already on the local network (perhaps via ARP spoofing), intercepts the victim's DNS query.
- **The Spoof:** The attacker quickly sends a fake DNS response to the victim. This fake response says, my-real-bank.com is at my IP address: ATTACKER_IP.
- **The Interception:** The victim's computer trusts this fake response and saves it in its DNS cache. When the victim tries to connect to their bank, they unknowingly connect directly to the attacker's server, which might host a perfect replica of the real banking site.
The attacker is now in the middle, capturing everything the victim types, including their username and password.

## Indicators of the Attack

We can look for the following key indicators while investigating the logs or network traffic for a potential Man-in-the-Middle attack using DNS spoofing.

- **Multiple DNS responses for the same query:** A legitimate resolver and a forged responder reply to the same query. This is the single most reliable indicator.
- **DNS response from an unexpected source:** A DNS reply arrives from an IP address that does not match any configured resolver (like 8.8.8.8 or your DNS server).
- **Suspiciously short TTL (Time-To-Live) values:** Attackers use very low TTLs (1 - 30s) to keep poisoned entries short-lived and reassert control.
- **Unsolicited DNS responses:** A DNS reply appears without a corresponding DNS request from the victim.

## Narrowing down DNS traffic

1) `dns`: Global search filter for dns packets
2) `dns.flags.response == 1 && ip.src == LEGIT-IP`: Filtering responses from the LEGIT IP to show valid DNS replies from the LEGIT DNS server
3) `dns.flags.response==1`: Filter to retrieve DNS responses
4) `dns && dns.qry.name == "LEGIT_DOMAIN"`: Filter DNS traffic for the domain of interest


## Spotting SSL Stripping in Action

SSL stripping is a man-in-the-middle technique in which an attacker intercepts and modifies traffic to remove or prevent TLS encryption between a client and a server. This causes the client to communicate over HTTP instead of HTTPS. The attacker retains a secure (HTTPS) session with the server while relaying plain HTTP to the victim, enabling eavesdropping and credential capture.

## How It Works

1) The victim initiates an HTTPS request to a website.
2) The attacker intercepts the request using ARP spoofing or a rogue access point.
3) The attacker connects to the website over HTTPS but relays the response to the victim through HTTP.
4) The victim unknowingly interacts over HTTP, exposing sensitive data in plaintext.

## Indicators of SSL stripping

- **Initial Request vs. Response:** The user's initial request may be for HTTPS (port 443), but the subsequent packets immediately shift to unencrypted HTTP (port 80) for the same domain.
- **Redirects/Link Rewriting:** Monitoring for redirects (HTTP Status Codes 301, 302) that persistently direct an HTTPS request to an HTTP resource.
- **Certificate Errors:** Although the attacker usually tries to hide this, the initial TLS/SSL Handshake may fail or display a self-signed certificate if the attacker uses a more direct proxying technique.

## Narrowing down SSL traffic

1) `tls || ssl`: Global search filter to inspect for SSL
2) `tls.handshake.type == 1 && tls.handshake.extensions_server_name == "LEGIT_DOMAIN"`: Filter to examine SSL traffic of the legitimate domain.
3) `dns.flags.response == 1 && ip.src == ATTACKERS_IP && dns.qry.name == "LEGIT_DOMAIN"`: Filter to isolate DNS responses from the attacker to show the victim was pointed to the attacker's IP.
4) `http && ip.src == VICTIMS_IP && ip.dst == ATTACKERS_IP`: Filter to check if victim is connected to attackers IP the stripped SSL traffic
