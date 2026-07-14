# Web Infrastructure

When you visit a website, your browser sends a request to a web server. The server processes the request, verifies access, and returns a response to the user. This response can be a webpage, an image, or data like search results or your account information. This request-response cycle is the foundation of how the web functions. Attackers can abuse this request-response cycle by overwhelming servers with requests, bypassing access controls, or even tricking the server into executing harmful commands.

## Web Infrastructure - Components of a Web Service

For example, any web service requires three main components to function.

- **Application:** The code, images, styles, and icons that dictate how the website looks and functions.
- **Web Server:** This component hosts the application. It listens for requests and returns a response to the user.
- **Host Machine:** The underlying operating system, Linux or Windows, that runs the web server and the application.

### Web Server

When you visit a website, your web browser sends a request to a web server. Web servers listen for incoming requests and return an appropriate response. Web servers are positioned in front of websites and applications, making them a crucial aspect of the internet's foundation. Because they are publicly exposed and handle all incoming web requests, web servers are a common target for attackers.

Here are some of the most common web servers that you will encounter.

- **Apache:** The most popular web server to host simple websites and blogs, most commonly WordPress(opens in new tab).
- **Nginx:** An industry standard for high-performance web apps. Used by companies like Netflix(opens in new tab), Airbnb, and GitHub.
- **Internet Information Services (IIS):** A Microsoft-developed web server commonly used in enterprise environments.

## Protecting the Web - Best Practices

**Protecting the Application**
- **Secure Coding:** Avoid insecure functions, ensure proper handling of errors, and remove sensitive information.
- **Input Validation & Sanitization:** Validate and sanitize user input to prevent injection attacks.
- **Access Control:** Restrict access based on user roles.

**Protecting the Web Server**
- **Logging:** Keep a detailed record of all web requests with access logs.
- **Web Application Firewall (WAF):** Filter and block harmful traffic based on defined rules.
- **Content Delivery Network (CDN):** Reduce direct exposure to your server and use integrated WAFs.

**Protecting the Host Machine**
- **Least Privilege:** Use low-privilege users for services.
- **System Hardening:** Disable unnecessary services and close unused ports.
- **Antivirus:** Add endpoint-level protection that blocks known malware.

**Security Tips for All Three Components**
- **Strong Authentication:** Don't just let anyone access your code, admin panels, or host machine.
- **Patch Management:** Ensure your app dependencies, web server, and host machine are up to date.

### Logging

Web servers can create logs for every request they receive. We call these access logs, and they are incredibly valuable from a security perspective because they track information about every interaction with the server, including the client's IP address, timestamp, requested page or data, response status from the server, and user agent. These fields can play an important role in investigations, helping analysts detect potential malicious activity and trace attacker behavior.

## Defense Systems - Content Delivery Network (CDN)

CDNs store and serve cached content from servers closer to the user to reduce latency. Imagine you have a main server housed in a central location. This main server provides information to edge servers worldwide so your customers can access data more quickly and safely. Aside from speed, CDNs also help in a security sense by acting as a buffer between the user and the origin server.

**Security Benefits**

- **IP Masking:** Hides the origin server IP address, which makes it harder for attackers to target.
- **DDoS Protection:** CDNs can absorb a large amount of traffic, making denial-of-service attacks less effective.
- **Enforced HTTPS:** Encrypted communication via TLS is enforced by default by most CDNs.
- **Integrated WAF:** Many CDNs, including Cloudflare CDN, Amazon CloudFront & Azure Front Door, integrate web application firewalls.
In essence, CDNs allow web apps to deliver data to customers more efficiently and securely.

### Web Application Firewall (WAF)

WAFs are a powerful tool that can be integrated as another layer of protection for websites and web applications. They inspect incoming HTTP traffic and block or log potentially harmful requests based on security rules. 
- **Cloud-based (Reverse Proxy):** Sits in front of the web server. These WAFs are easy to deploy and have great scalability.
- **Host-based:** Software deployed directly on the web server and offers control for each application.
- **Network-based:** A physical or virtual appliance situated on the network perimeter. More suited for enterprise environments.

### Some methods used by WAFs to inspect HTTP requests 

1) **Signature-Based Detection:** Matches known attack patterns or payloads
**Example:** A request with a User-Agent that matches a known tool, sqlmap/1.8.1

2) **Heuristic-Based Detection:** Analyzes the context and behavior of requests
**Example:** A long query string with special characters search?q=%3Cscript%20(1)

3) **Anomaly & Behavioral Analysis:** Flag deviations from normal traffic behaviour
**Example:** A single IP address makes repeated login attempts in a short period of time

4) **Location & IP Reputation Filtering:** Uses location and threat intel to block IPs
**Example:** A request from an IP address that is outside of your normal business area

### Antivirus (AV)

AVs are often misunderstood as a blanket protection measure, but they are primarily made to safeguard endpoints, such as desktops, laptops, and servers, from known malicious files and programs. Most AVs rely on signature-based detection, which means they compare files with a database of known malware or patterns.

While web attacks usually target the application layer, not the host machine, AVs still play an important role in host protection. They can help detect malicious file uploads, such as web shells, post-exploitation tools, and other malicious software. AVs are just one layer in a broader defense-in-depth strategy and should be combined with other security measures to provide stronger protection.

# Client-Side Attacks 

Client-side attacks rely on abusing weaknesses in user behavior or on the user's device. These attacks often exploit vulnerabilities in browsers or trick the user into performing unsafe actions to gain access to accounts and steal sensitive information.

## Common Client-Side Attacks

- **Cross-Site Scripting (XSS)** is the most common client-side attack, in which malicious scripts are run in a trusted website and executed in the user's browser. If your website has a comment box that doesn't filter input, an attacker could post a comment like: Hello `<script>alert('You have been hacked');</script>`. When visitors load the page, the script runs inside their browser, and the pop-up appears. In a real attack, instead of a harmless pop-up, the attacker could steal cookies or session data.
- **Cross-Site Request Forgery (CSRF):** The browser is tricked into sending unauthorized requests on behalf of the trusted user.
- **Clickjacking:** Attackers overlay invisible elements on top of legitimate content, making users believe they are interacting with something safe.

# Server-side Attacks

Server-side attacks rely on exploiting vulnerabilities within a web server, the application's code, or the backend that supports a website or a web application. While client-side attacks manipulate users' interaction with a site, server-side attacks focus on taking advantage of the systems themselves. By exploiting flaws and vulnerabilities, server logic, misconfigurations, or input handling, attackers can gain access, steal information, and cause damage to running services.

## Catching Server-side Attacks

One advantage for defenders when dealing with server-side attacks is that they leave a trail of evidence, if you know where to look. Every web request sent to an application is processed by the server and is recorded in logs or other monitoring systems. These requests also travel across the network, meaning network traffic can reveal suspicious behavior.

## Common Server-Side Attacks

- **Brute-force** attacks occur when an attacker repeatedly attempts different usernames or passwords in an attempt to gain unauthorized access to an account. Automated tools are often used to send these requests quickly, allowing attackers to go through large lists of credentials and common passwords. T-Mobile(opens in new tab) faced a breach in 2021 that stemmed from a brute-force attack, allowing attackers access to the personally identifiable information (PII) of over 50 million T-Mobile customers.
- **SQL Injection (SQLi)** relies on attacking the database that sits behind a website and occurs when applications build queries through string concatenation instead of using parameterized queries, allowing attackers to alter the intended SQL command and access or manipulate data. In 2023, an SQLi vulnerability in MOVEit(opens in new tab), a file-transfer software, was exploited, affecting over 2,700 organizations, including U.S. government agencies, the BBC, and British Airways.
- **Command Injection** is a common attack that occurs when a website takes user input and passes it to the system without checking it. Attackers can sneak in commands, making the server run them with the same permissions as the application.

# Log-Based Detection

Logs can be a valuable tool for detecting web attacks. Every request sent to a web server can leave evidence in its access and error logs. Defenders can spot patterns that reveal scanning, exploitation attempts, or other attacks by reviewing log entries.

## Log Fields and their example indicators

1) **Client IP Address:** A known malicious or outside of the expected geo range
2) **Timestamp and Requested Page:** Requests made at unusual hours or repeated in a short period of time
3) **Status Code:** Repeated 404 responses indicating a page could not be found
4) **Response size:** Significantly smaller or larger than normal response sizes
5) **Referrer:** Referring pages that don't fit normal site navigation
6) **User-Agent:** Outdated browser versions or common attack tools (e.g. sqlmap, wpscan)

# Network-Based Detection

Network traffic analysis allows analysts to examine the raw data exchanged between a client and a server. By capturing and inspecting packets, analysts can observe attack behavior on a more detailed level, including the underlying transport protocols and the application data itself.

# Web Application Firewall

Web Application Firewalls (WAFs) are often the first line of defense for websites and web applications.

## Rules

WAFs inspect and decide whether to allow a web request or block it entirely based on predefined rules.
A few categories of firewall rules include:

1) **Block common attack patterns:** Blocks known malicious payloads and indicators
**Example:** Block malicious User-Agents: sqlmap

2) **Deny known malicious sources:** Uses IP reputation, threat intel, or geo-blocking to stop risky traffic
**Example:** Block IPs from recent botnet campaigns

3) **Custom-built rules:** Tailored to your specific application’s needs
**Example:** Allow only GET/POST requests to /login

4) **Rate-limiting & abuse prevention:** Limits request frequency to prevent abuse
**Example:** Limit login attempts to 5 per minute per IP

# Detecting Web Shells - Web Shell Overview

A web shell is a malicious program uploaded to a target web server, enabling adversaries to execute commands remotely. Web shells often serve as both an initial access method (via file upload vulnerabilities) and a persistence mechanism.
Once access has been gained on a compromised server, attackers can use a web shell to move through the kill chain, performing reconnaissance, escalating privileges, moving laterally, and exfiltrating data.`

## Web Shell Deployment

For an attacker to upload and execute a web shell, a file upload vulnerability, misconfiguration, or prior access to the system is required. These vulnerabilities arise when an application fails to validate file type, extension, content, or destination. While web shells are used as an initial access vector, they can also serve as a persistence mechanism if the attacker has already compromised the system and wants to maintain long-term access.

# Anatomy of a Web Shell

**Legitimate Function Abuse:**
Web shells rely on the abuse of legitimate functions within programs.
System execution functions in PHP, such as `shell_exec()`, `exec()`, `system()`, and `passthru()`, can be abused to gain command execution.

# Log-Based Detection

Web shells rely on the abuse of web servers, so web server logs are a natural place to start our hunt for evidence. Understanding the difference between normal and suspicious behavior can help uncover malicious activity.

**The remote log name field is typically represented by a hyphen (-), as it is a legacy field that is rarely used today. However, it still appears in access logs for compatibility. Similarly, the authenticated user field is usually shown as a hyphen as well, unless the server required prior authentication, in which case it may contain the actual username.**

## Web Indicators

**Unusual HTTP Methods & Request Patterns**

- Repeated GET requests in quick succession could mean an attacker is probing for a valid place to upload a shell
- POST requests to valid upload locations following repeated GET requests
- Repeated GET or POST requests to the same file could indicate web shell interaction

### Request methods to be aware of

1) **GET:** Retrieve a resource
**Possible Abuse:** Used for recon or interacting with a web shell

2) **POST:** Submit data to the server
**Possible Abuse:** Upload or interact with a web shell

3) **PUT:** Upload or replace a file on the server
**Possible Abuse:** Upload a web shell

4) **DELETE:** Remove a resource from the server
**Possible Abuse:** Cleanup methods

5) **OPTIONS:** Request methods that are supported
**Possible Abuse:** Reconaissance

6) **HEAD:** Similar to GET but only returns headers
**Possible Abuse:** To detect files

**Suspicious User-Agents & IP Addresses**
The User-Agent identifies the client making requests to the web server and provides information about the browser, device, and operating system.

- Altered User-Agents: Mozilla/4.0+(+Windows+NT+5.1) shortened to Mozilla/4.0
- Outdated User-Agents: Mozilla/4.0 (compatible; MSIE 6.0) MSIE 6.0 released in 2001
- Blacklisted User-Agents: curl/1.XX.X or wget/1.XX.X for example
- Suspicious IP Addresses: A network normally only sees internal traffic so an outside IP address would be suspicious

**Query Strings**
Part of the URL that associates values with a parameter. example.php?query=somequery

- Abnormally long or suspicious query strings, especially containing keywords like cmd= or exec= 
- Encoded query strings. ?query=whoami becomes ?query=d2hvYW1p when Base64 encoded. Cyberchef is an excellent tool for decoding Base64 and many other forms of encoding and obfuscation.

**Missing Referrer**
The referrer shows the URL the users visited before being linked to the current page.

- A missing referrer can be potentially indicative of web shell activity
- There are valid reasons why a referrer might be missing (e.g., browsers blocking them for privacy, if a URL is directly accessed)

**Sample suspicious web request** including some of the above indicators.

1) Known malicious or untrusted IP address.
2) Abnormal timestamp. Perhaps outside of normal business hours.
3) POST request with a search query string to a malicious file.
4) No referrer. So this page was accessed directly. (not always a valid indicator)
5) A suspicious User-Agent string that is not typically associated with a web browser.

## Auditd

A native Linux utility that tracks and records events, creating an audit trail. Rules can be created for `auditd`, which determine what is logged in the `audit.log`. Rules can be highly configured to match specific conditions, such as when certain programs are run or files are modified in a particular directory.

## Web & Auditd Correlation

Detecting web shells effectively requires correlating multiple log sources. Combining web access and error logs with auditd provides more insight and can confirm if a file was created, modified, or executed, and by which user or process. A suspicious POST request in web logs can be linked to an audit event that includes a creat or execve syscall, showing a script wrote a file or ran commands. Combining this information helps us build a clearer picture of the attack sequence.

## Leverage SIEM Platforms

Some benefits of Security Information and Event Management (SIEM) platforms include: 

- Centralized log collection and correlation, which is especially useful when dealing with many different log types.
- Targeted queries can be created to uncover signs of malicious activity, including web shells.
- Allows analysts to search and analyze logs more efficiently.

# File System Analysis

An attacker's web shell must be stored somewhere. Analyzing web server files is crucial in identifying uploaded web shells or locating files modified to include a web shell payload.
It should be noted that some platforms like WordPress and Django store page content in a database rather than a file system, so malicious code may be injected into posts, themes, or settings and won't appear in normal file system searches.

Here are some common web server directories where web shells are typically placed:

- Apache: /var/www/html/ Default on most Linux distros
- Nginx: /usr/share/nginx/html/ Default on many Linux setups

**Suspicious or Random File Names**
Can be used by attackers to evade detection. Be on the lookout for names that deviate from standard application files. 

- Monitor files with executable extensions like .php & .jsp
- Be on the lookout for double extensions to disguise malicious files image.jpg.php

**Helpful Commands**
You can use `find` to search for recently modified scripts. Another helpful tool, `grep`, can be used to track down suspicious functions like `eval(` within files.

## Network Traffic Analysis

Network traffic analysis allows analysts to go beyond logs by examining the data exchanged between a client and a server. By inspecting packet payloads, it becomes possible to observe attacker behavior on a more detailed level.

Many of the indicators that analysts need to be on the lookout for in log analysis can be applied to network traffic analysis as well.

- Unusual HTTP Methods & Request Patterns
- Suspicious User-Agents & IP Addresses
- Encoded Payloads
- Malicious Code or Commands in Request Bodies
- Unexpected Protocols or Ports
- Unexpected Resource Usage
- Web Server Processes Spawning Command Line Tools

Some useful Wireshark http filters.

- `http.request.method == “METHOD”`
Hunting for repeated or unusual requests can be useful
- `http.request.uri contains “.php”`
Can be helpful in finding suspicious or modified files
- `http.user_agent`
Used to locate unusual or outdated User-Agents

# Detecting Web DDOS - DDOD and DDOS attacks

Denial-of-Service (DoS) attacks can take many forms, but their ultimate aim is to disrupt or completely block access to a website or web service. At their core, Denial-of-Service (DoS) attacks are meant to overwhelm a website or app so that people can’t use it. When this happens, customers can’t log in, shop, or access services, and businesses lose money and trust.

## Distributed Denial-Of-Service (DDOS)

The limitation of a basic DoS attack is that it relies on a single machine and a single internet connection. While one computer can generate many requests, its impact is capped by its CPU, memory, bandwidth, and network.
To scale up, attackers turn to Distributed Denial-of-Service (DDoS) attacks and utilize botnets, an army of compromised devices under their control. The computers, IoT devices, and even servers are often infected with malware and secretly controlled by the attacker. When instructed, the bots flood the target website or web application with traffic, overwhelming it much more effectively than a single machine could.

## Types of Denial-Of-Service Attacks

1) **Slowloris:** Sending many partial HTTP requests to tie up server resources
2) **HTTP Flood:** Sending a large number of HTTP requests to overwhelm the server
3) **Cache Bypass:** Bypassing CDN edge servers and forcing the origin server to respond
4) **Oversized Query:** Forcing the server to process large, resource-intensive requests
5) **Login/Form Abuse:** Overloading authentication logic with login attempts or password resets
6) **Faulty Input Validation Abuse:** Exploiting poorly designed input handling

## Attack Motives

1) **Financial Loss:** Disrupt services to stop or reduce sales and revenue
**Example:** Flooding an e-commerce website during peak holiday sales

2) **Extortion:** Demand payment to stop a current attack
**Example:** Threatening a bank with a ransom DDoS

3) **Hacktivism:** Disruption for social or political protest
**Example:** Attacking government websites during election season

4) **Distraction:** Redirect defenders' attention while other attacks take place
**Example:** Launching a DDoS while attacking other infrastructure

5) **Competition:** Disrupt a rival's service to drive up their costs or gain market share
**Example:** A competitor launches a DDoS during a product launch

6) **Denial of Wallet:** Force the victim to rack up service usage costs
**Example:** Attackers repeatedly access AWS S3 data, generating costs per request

7) **Reputational Damage:** Cause customers to lose trust in a company
**Example:** Game servers crashing during launch day

## Indicators of DOS and DDOS Attacks

1) **High Request Rate:** A resource-heavy page like /login is flooded with requests to overwhelm authentication processes. Login pages are common targets since each request may trigger password checks and database queries
**Example:** 10.10.10.100 → 1000 GET /login

2) **Odd User-Agents:** Attackers spoof outdated or unusual User-Agents to blend in or bypass filters. Spotting traffic with tools like curl or Python-urllib/3.x, for example, can be a red flag for automated attacks
**Example:** curl/7.6.88 → /index repeatedly

3) **Geographic anomalies:** Legitimate traffic typically comes from a few regions where real users are located. A globally distributed botnet may utilize IP addresses from around the world
**Example:** IP address origins dotted around the world

4) **Burst Timestamps:** A sudden spike of requests packed into the same second creates an unnatural traffic pattern that points to automation
**Example:** 50 requests in 1 second → /search

5) **Server Errors (5xx):** A sudden surge of server error responses (500 - 511) indicates resources are maxed out and the service is struggling under attack traffic
**Example:** A significant spike of 503 Service Unavailable errors

6) **Logic Abuse:** Attackers craft queries that overload the server, forcing it to load huge amounts of information and slowing it down for everyone
**Example:** GET /products?limit=999999

## Targeted Resources

Commonly targeted endpoints and reasoning: 

- `/login` - involves authentication processes 
- `/search` - requires complex database queries
- `/api` endpoints - critical for dynamic content delivery
- `/register` or `/signup` - requires database writes and validation
- `/contact` or `/feedback` - requires database entries and can trigger email notifications
- `/cart` or `/checkout` - requires session management, inventory checks, and payment processing

## Defense - Application Level Defense

- **Secure Development Practices**

A secure site starts with secure code. Search fields and forms must validate input so they can’t be abused. Think of a search form like a librarian who looks up books on request. If the librarian has clear rules, like “only search for titles under 50 characters”, they can respond quickly. Without rules in place, someone could ask the librarian to search for an overly long or strange title with special characters, slowing them down and delaying everyone else's requests. In the same way, web applications need input validation to stop attackers from submitting specialized queries aimed at overloading the system.

- **Challenges**

One way to stop automated traffic is to require a challenge before granting access. This could be a CAPTCHA, where the user solves a puzzle, like clicking images or checking a box. For humans, it’s a small step, but for bots, it can block or slow down an attack.
Websites can also use JavaScript challenges, which run quietly in the background to confirm if a visitor is a real user or automated traffic. Legitimate users usually don’t notice them, but automated tools and botnets often fail, making these challenges an effective filter against malicious traffic.

## Defense - Network and Infrastructure Defenses

**Content Delivery Network (CDN)**

CDNs help manage server load by caching and serving content from edge servers closest to users. This reduces latency and allows the origin server to handle only a fraction of requests, while the CDN serves the majority. As a result, CDNs take on much of the burden of mitigating DDoS attacks. They also provide load-balancing to distribute traffic across servers, ensuring no single server is overloaded and rerouting requests if one becomes unavailable.

**Web Application Firewall (WAF)**

CDNs typically integrate WAFs in an effort to shield their customers' servers. They inspect incoming traffic and either allow, challenge, or block requests. WAFs work off of rules that integrate known attack indicators and threat intelligence. Modern WAF solutions are already very good at mitigating DoS and DDoS attacks because they know what to look out for. Custom rules can also be developed to assist in defending against targeted threats.

For example, you might implement a rate-limiting firewall rule that limits requests to /login.php to five per minute. If the originating IP requests the page more than five times, it would be blocked from making future requests for a period of time or provided with a challenge to prove it is a human-made request. 

## Bypassing Security Measures

While CDNs, caching, and WAFs provide strong protection, attackers often attempt to bypass these defenses. One technique is appending random query parameters. Your CDN might serve a cached URL at `/products`, but if an attacker appends the query with a random string like `/products?a=abcd`, your CDN cannot serve the cached page, and the origin server is forced to respond. Similarly, changing user agents, spoofing referrer pages, or launching requests from diverse geographic regions can help attackers evade WAF filtering rules.