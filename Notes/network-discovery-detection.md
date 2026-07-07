# Attackers and Network Discovery

Attackers start their attack by trying to discover an organisation's assets that are open to the publicly accessible internet (called the attack surface). During this discovery phase, the attacker attempts to ascertain some of the following information:

- What assets can be accessed by the attacker? 
- What are the IP addresses, ports, OS, and services running on these assets?
- What versions of services are running? Does any service have a vulnerability that can be exploited?
In short, the attacker is trying to find an opening that will allow them to exploit the network. 

# Defenders and Network Discovery

Defenders also sometimes run software that performs network discovery activity. During this activity, defenders want to achieve the following goals:

- Inventory the organisation's assets and ensure all assets are documented.
- Ensure no unnecessary IP, port, or service is open, and whatever is running is necessary.
- Ensure vulnerabilities are patched, or at least the exploitable vulnerabilities are patched.
In short, defenders attempt to reduce the attack surface as much as possible.

# The Challenge in Detecting Network Discovery

Both attackers and defenders perform discovery actions. Multiple research organisations, web crawlers, search engines, etc., also perform similar discovery actions to map the resources present on the Internet. SOC teams must differentiate between good and bad discovery. To mitigate this challenge, SOC teams often use the following techniques.

- Allowlist known internal and benign external scanners, ensuring no alerts are triggered on those sources.
- Integrate Threat Intelligence with detection use cases and flag scanning activity only from known malicious or suspicious sources.
- Since the previous point has a chance of missing some malicious scanning activity, some teams use the Threat Intelligence to raise the severity of the alerts instead of only triggering alerts on them. In addition, they add some generic use cases to alert on scanning behavior, which we will learn about in the next tasks.

# External Scanning Activity

A SOC analyst might encounter scanning activity from outside their organisation's network and scan the machines inside the network, mainly the public-facing assets on the perimeter. The SOC analyst will observe that the source IP in this type of attack is an external IP and the destination is an IP address belonging to the organisation. This type of scan indicates that the attack is still in the Reconnaissance(opens in new tab) phase of the MITRE ATT&CK lifecycle. The attacker does not have a foothold inside the network and is doing initial reconnaissance to identify opportunities to gain initial access to the network.
This type of scanning is in the initial phases of the attack, and the attacker hasn't yet achieved any foothold in the network, so it is a low-severity type of scanning. In response to an external scanning activity, a SOC analyst can block the offending IP addresses on the perimeter firewall of the organisation's network. However, we must note that the attacker might come back again by masking their IP address.

# Internal Scanning Activity

The other type of scanning that a SOC analyst might observe is an internal-to-internal scanning activity. The SOC analyst will observe that the source and destination IP addresses are private IP addresses inside the organisation's network. This type of scan initiates inside the organisation's network and scans assets from the same network. This type of scan indicates that the attack has progressed to the Discovery(opens in new tab) phase of the MITRE ATT&CK lifecycle. In some other frameworks, it might also be called internal reconnaissance, indicating that the attacker has a foothold in the network and is now gearing up for lateral movement. 
Since this type of scanning indicates that the attacker is already inside the network, it is a high-severity alert. After ensuring this is not some authorised activity, a SOC analyst will escalate this alert and initiate the Incident Response process. In this case, merely blocking the source IP on the firewall will be insufficient, and a deeper investigation into the system will be required, and root cause analysis will have to be performed.

# Horizontal vs Vertical Scanning

Once the attackers know what hosts are present on a network, they often want to identify what ports are open on these hosts. This is called a port scan, and can be of the following types:

## Horizontal Scanning

Sometimes, an attacker will scan for the same port across multiple destination IP addresses. This type of scan is called a horizontal scan. Horizontal scans are performed to identify which hosts expose a particular port. An attacker might perform this scan if they intend to exploit that specific port. An example can be the WannaCry ransomware, which spread through the network using an SMBv1 vulnerability and scanned for machines with port 445 (which is used for SMB) open. 
We can detect a horizontal scan in the logs if we see the same source IP, a single destination port, but multiple destination IP addresses across multiple events.

## Vertical Scanning

Vertical scanning occurs when a single host IP address is scanned across multiple ports. Attackers perform vertical scanning to footprint a host and identify its exposed services. This activity might be performed when an attacker is focused on identifying a vulnerability on a single machine because they consider it a valuable target based on their objectives. For example, if an organisation exposes only a single server to the internet, any attacker who wants to breach that organisation would first perform a vertical scan on that server to identify open ports and understand the services running on the machine.
We can detect a vertical scan in the logs if we observe the same source IP, the same destination IP, but multiple destination ports across multiple events.

## The Mechanics of Scanning (Ping Sweep)

This is one of the most basic network scanning techniques. Ping sweeps are generally used to identify hosts present (and online) on a network. This scan is run by sending an Internet Control Message Protocol (ICMP) packet to the host. If the host is online, it will reply with an ICMP packet of its own. However, it is often blocked by security controls in some organisations nowadays, making it easier to defeat this type of scanning activity.

## TCP SYN Scans

A TCP connection is initiated by a three-way handshake, following the steps SYN, SYN-ACK, ACK to establish the connection. Network scanners can sometimes use this functionality of the TCP handshake to identify online hosts and their open ports. The scanner sends a SYN request to the recipient. If a SYN-ACK response is received, it means that the host is online and the port on which the SYN connection was sent is also open. This is a stealthy scan that often blends in with the rest of the network traffic and is harder to detect.

## UDP Scan

Another way to identify online hosts and open ports is by sending a (usually empty) UDP packet. If the port is closed, the host sends back an ICMP port unreachable reply. This signifies that the port is closed, but the host is online. In some cases, the scanner will not receive any response until a set timer is reached. If no response is received, the scanner will mark the port as open (but this is not a clear evidence of the port being open). In rare cases, the scanner might receive a UDP packet in response, which is evidence of the port being open. As we can infer, a UDP scan is unreliable and slow as it relies on waiting until it does not receive any response till a timeout is reached.

Most organisations often perform internal scans to check for vulnerabilities, keep an eye on rogue assets, and identify any opportunities for attack surface reduction. It is pertinent for a SOC analyst to know the details of these scans, such as the IP address from where they run, the type of scanning being performed, and any scanning schedules. Ideally, these scans should be excluded from any detection use cases of the SOC team to reduce noise.

