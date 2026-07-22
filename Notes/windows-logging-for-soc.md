# Security Log: Authentication

Two most important Security logs: Successful Logon (4624) and Failed Logon (4625)

1) **Event ID 4624 (Successful Logon):** Detect Suspicious RDP/network logins and identify the attack starting point
2) **Event ID 4625 (Failed Logon):** Detect brute force, password spraying or vulnerability scanning

## How to Detect RDP Brute Force 

1) Open Security logs and filter for 4625 event ID (Failed login attempts)
2) Look for events with Logon Type 3 and 10 (Network and RDP logins)
    - For most modern systems, the logon type will be 3 (since NLA(opens in new tab) is enabled by default)
    - For older or misconfigured systems, the logon type will be 10 (since NLA is not used)
3) Every event is now worth your attention, but the main red flags are:
    - Many attempted users like admin, helpdesk,  and cctv (Indicates password spraying)
    - Many login failures on a single account, usually Administrator (Indicates brute force)
    - Workstation Name does not match a corporate pattern (e.g. kali instead of THM-PC-06)
    - Source IP is not expected (e.g. your printer trying to connect to your Windows Server)

## How to Analyse RDP Logons

1) Open Security logs and filter for 4624 event ID (Successful logins)
2) Look for events with Logon Type 10 (RDP logins)
    - If NLA(opens in new tab) is enabled, every RDP logon event is preceded by another 4624 with logon type 3
    - To get a real Workstation Name, you need to check the preceding logon type 3 event
3) Your red flags are either a preceding brute force or a suspicious source IP / hostname
4) If you assume that the login was indeed malicious, find out what happened next:
    - Windows assigns a Logon ID to every successful login (e.g. 0x5D6AC)
    - Logon ID is a unique session identifier. Save it for future analysis!

# Security Log: User Management

Below is a breakdown of the common event IDs you can use:

1) **Event ID 4720/4722/4738 (User account created/enabled/changed):** Attackers might create a backdoor account or even enable an old one to avoid detection 
2) **Event ID 4725/4726 (User account disabled/deleted):** In some advanced cases, threat actors may disable privileged SOC accounts to slow down their actions
3) **Event ID 4723/4724 (User password changed/reset):** Given enough permissions, threat actors might reset the password and then access the required user
4) **Event ID 4732/4733 (User added/removed from a security group):** Attackers often add their backdoor accounts to privileged groups like "Administrators"

## How to Hunt for Backdoored Users

1) Open Security logs and filter for 4720 / 4732 event IDs
2) Manually review every event; your red flags are:
    - No one from your IT department can confirm the action
    - Changes were made during non-working hours or on weekends
    - The subject user's name is unknown or unexpected to you (e.g. "adm.old.2008" creating new Windows users)
    - The target user's name does not follow a usual naming pattern (e.g. "backup" instead of "thm_svc_backup")
3) If you confirmed that the action was malicious, find out the login details:
    - Copy the Logon ID field from your 4720 / 4732 event
    - Find the corresponding login event with the same Logon ID
    - Refer to the workbooks from the previous task for further analysis

# Sysmon: Process Monitoring

Two ways to enable process monitoring on Windows:

1) **Event ID 4688 (Security Log: Process Creation):** Log an event every time a new process is launched, including its command line and parent process details
2) **Event ID 1 (Sysmon: Process Creation):** Replace 4688 event code and provide more advanced fields like process hash and its signature

## How to Analyse a process launch

1) Open Sysmon logs and filter for event ID 1
2) Review the fields from the process and binary info groups. The red flags are:
    - Image is in an uncommon directory like C:\Temp or C:\Users\Public
    - Process is suspiciously named like aa.exe or jqyvpqldou.exe
    - Process hash (MD5 or SHA256) matches as malware on VirusTotal(opens in new tab)
3) Review the fields from the parent process group. The red flags are:
    - Parent matches red flags from step 2 (suspicious name, path, or hash)
    - Parent is not expected (e.g. Notepad launching some CMD commands)
4) If still in doubt, go up the process tree until you are confident in your verdict:
    - Find the preceding event where ProcessId equals ParentProcessId in your event
    - Analyze it by following steps 2 and 3 (suspicious parent, name, path, or hash)
5) Finally, trace the attack chain by filtering all Security and Sysmon events with the same Logon ID

# Sysmon: Files and Network

1) **Event ID 11/13 (File Create/Registry Value Set):** 4656 for file changes and 4657 for registry changes, both disabled by default. Detect files dropped by malware or its changes to the registry (e.g. for persistence) 
2) **Event ID 3/22 (Network Connection/DNS Query):** No direct alternative, requires additional firewall and DNS configuration. Detect traffic from untrusted processes or to known malicious destinations. 

## How to Analyse Process Activity

1) Copy the ProcessId field from the event ID 1
2) Search for other Sysmon events with the same ProcessId
3) Your red flags for network connection events are:
    - Connection to external IPs on port 80 or on non-standard ports like 4444
    - Connection to known malicious IPs (e.g. by checking on VirusTotal)
    - DNS queries to suspicious domains (*.top, *.click, or hpdaykfpadvsl.com)
4) Your red flags for file and registry changes are:
    - Files dropped to staging directories like C:\Temp or C:\Users\Public
    - Dropped file is a script (.bat or .ps1) or an executable file (.exe or .com)
    - Created files or registry keys are used for persistence
    
# Powershell: Logging Commands

PowerShell is a powerful tool built into Windows that attackers love to abuse. Mainly because it is both trusted and capable of malware download, system discovery, data exfiltration, and even advanced techniques like process injection. However, you won't capture its commands by just using process creation logs like the Sysmon event ID 1.

## Powershell History File

The PowerShell history file is a plain text file automatically created by PowerShell. It simply records every command you type into a PowerShell window and is immediately updated when you press Enter to submit a command.
- The history file is very useful for tracking malicious actions like system discovery or malware download
- The history file is created for every user, meaning that you may see five files if there are five active system users
- It survives system reboots unless manually deleted and saves all PowerShell commands entered for all time
- It does not log command outputs and does not show script content (e.g. when running powershell .\script.ps1)