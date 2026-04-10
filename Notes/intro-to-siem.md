# What is a SIEM?

Security Information and Event Management System (SIEM) is the core security solution that a SOC analyst uses in the security Operations center.

# Log Everywhere

Multiple devices in a network communicate with each other and most of the time, with the internet through a router.
These devices continuously generate logs of the activities that occur within them. We can also call these devices log sources. The logs they generate serve as a trail of all the activities and are extremely helpful for identifying malicious activities or general troubleshooting. They are mainly divided into two categories which are:

#### Host-Centric Log Sources

Host-centric log sources are logs generated from individual endpoints such as servers and workstations. They include operating system logs, EDR telemetry, authentication logs and application logs andprovide detailed visibility into activities occurring within a host.
These log sources capture events that occurred within or related to the host. Examples of host centric logs are:
- A user accessing a file
- A user attempting to authenticate
- A process execution activity
- A process adding/editing/deleting a registry key or value
- Powershell execution

#### Network-Centric Log Sources

Network-related logs are generated when the hosts communicate with each other or access the intenet to visit a website. Devices that generate network-centric logs are firewalls, IDS/IPS, routers, etc. Examples of network-centric logs are:
- SSH connection
- A file being accessed via FTP
- Web traffic
- A user accessing the company's resources through VPN
- Network file sharing Activity

# Answers Nowhere

There are some challenges that comes with the generated logs from their sources:
- **Numerous Log Sources**: A network has many log sources, which generate hundreds of events per second. These logs are scattered across different devices, and examining the logs on each device one by one in case of an incident can be tedious.
- **No Centralization**: As logs reside on the machines on which they are generated, you may need to connect with each log source via SSH, RDP, etc, to analyze logs from multiple log sources. This is very inefficient and can waste a lot of your valuable time during the investigations.
- **Limited Context**: Individual logs cannot tell the whole story of an activity. During any incident, the individual activities on different log sources may seem harmless. But if these logs are correlated, they can indicate a whole different story. For instance, you observed a file access event in a system, which is generally normal activity. However, if you correlate different log sources, you might come to know that this file was accessed by a user who accessed this machine through lateral movement after compromising another machine in the network.
- **Limited Analysis**: The log sources generate numerous logs per second, and analyzing all the logs from all the devices manually to identify any abnormal activity is nearly impossible for humans. Realistically, the analysts will miss a lot of important logs in between the analyses due to their huge number.
- **Format Issues**: Different log sources generate logs in various formats. Analysts need to know all these formats to analyse them, which can be extremely difficult, especially when dealing with numerous log sources in a network.

# Why SIEM

Security Information and Event Management (SIEM) is a security solution that collects logs from various types of log sources, standardizes their format into a consistent one, correlates them and detects malicious activities using detection rules.
![Image](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/63724f4da84dd3cfbaf2937790910e20.png)

## Features of SIEM

- **Centralized Log Collection**: SIEM collects logs from all sources (Endpoints, servers, firewalls, etc.) and centralizes them in one place. These logs are pulled through lightweight agents or APIs and populated into the SIEM solution. This solves the problem of jumping on every machine individually to analyse its logs.

- **Normalization of Logs**: Raw logs are of different formats and sizes. A windows log does not look the same as a Linux log. It also ensures that all the logs are broken down into different fields and presented in one consistent format. Breaking down a log into several field for ease of understanding is known as Parsing, and converting all the logs of various log sources into one consistent format is known as Normalisation.

- **Correlation of Logs**: Individual logs are not very useful. SIEM correlates the logs of different sources and finds any relationship between them. This helps to identify malicious activity by analyzing its pattern.

- **Real-time Alerting**: SIEM detects malicious activities based on the rules it contains. Many rules come with a SIEM by default. However, Analysts make new detection rules based on their requirements to mature future detections. When the conditions for these detection rules are satisfied, alerts are triggered, and the analysts are notified. Analysts can then investigate these alerts within the SIEM platform.

- **Dashboards and Reporting**: Dashboards are the most important components of any SIEM. The summary of this analysis is presented in the form of actionable insights with the help of multiple dashboards. Some information that can be found in a dashboard:
    * Alert Highlights
    * System Notification
    * Health Alert
    * List of Failed Login Attempts
    * Events Ingested Count
    * Rules triggered
    * Top Domains Visited

# Log Sources

Every device in the network generates some kind of log whenever an activity is performed on it, such as a user visiting a website, connecting to SSH, logging into their workstation, etc. 

## Windows Machine

Windows records every event that can be viewed through the Event Viewer. It assigns a unique ID to each type of log activity, making it easy for the analyst to examine and keep track of.

## Linux Machine

Linux OS stores all the related logs, such as events, errors, warnings, etc. These are then ingested into SIEM for continuous monitoring. Some common locations where Linux stores logs are:
- **/var/log/httpd**: Contains HTTP Request /Response and error logs
- **/var/log/cron**: Events related to cron jobs are stored in this location
- **/var/log/auth.log and /var/log/secure**: Stores authentication-related logs
- **/var/log/kern**: This file stores kernel-related events

## Web Server

It is important to monitor all requests/responses coming in and out of the web server for any potential web attack attempt. In Linux, common locations to write all apache-related logs are **/var/log/apache** or **/var/log/httpd**.

# Log Ingestion

All these logs provide a wealth of information and can help identify security issues. Some common methods used by SIEM to ingest the logs are:
1. **Agent/Forwarder**: These SIEM solutions provide a lightweight tool called an agent (forwarder by splunk) that gets installed on the Endpoint. It is configured to capture and send all the important logs to the SIEM server.
2. **Syslog**: Syslog is a widely used protocol to collect data from various systems like web servers, databases, etc and send real-time data to the centralized destination.
3. **Manual Upload**: Some SIEM solutions, like splunk, ELK, etc, allow users to ingest offline data for quick analysis. Once the data is ingested, it is normalised and made available for analysis.
4. **Port-Forwarding**: SIEM solutions can also be configured to listen on a certain port, and then the endpoints forward the data to the SIEM instance on the listening port.

# Alerting Process and Analysis

SIEM solution has detection rules that catch threats. These rules play an important role in the timely detection of threats, allowing analysts to take action on time.
>Detection rules are logical expressions set to be triggered.
A few examples are:
- If a user gets five login attempts in 10 seconds, raise an alert for **_Multiple Failed Login Attempts_**
- If login is successful after multiple failed login attempts, raise an alert for **_Successful Login After multiple Login Attempts_**
- A rule is set to alert every time a user plugs in a USB (Useful if USB is restricted as per the company policy)
- If outbount traffic is > 25MB, raise an alert to potential data exfiltration Attempt (Usually, it depends on the company policy)

