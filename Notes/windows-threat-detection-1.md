# Intro to Initial Access

No matter what the final goal is, the first step of a threat actor is to get through the front door, and the moment an attacker successfully gets in is known as Initial Access.
Different types of Initial Access methods which are: those requiring an exposed service and those relying on human interaction.

## Exposed Services

Putting a Windows server directly on the Internet is a common task for IT teams - corporate websites require an open HTTP port to show content, a mail server can't handle emails without an active SMTP port, and IT admins need RDP to manage the machine remotely. However, every exposed service introduces major security risks. Within minutes, your exposed system can be scanned by automated bots looking for open ports, weak passwords, or unpatched vulnerabilities. And if something is not protected enough, threat actors will use their chance,  as proven by these MITRE techniques:

- T1133 / External Remote Services: Threat actors will look for exposed RDP/VNC/SSH with weak passwords to get remote access to the machine
- T1190 / Exploit Public-Facing Application: Threat actors will look for misconfigured or vulnerable websites and applications

## User-Driven Methods

Unless you help the threat actors yourself, it is very hard to infect your laptop. However, people often help threat actors by clicking on malicious links, launching phishing attachments, using pirated software, picking up unknown USB devices, and so on. And since humans are still the weakest link in cyber security and Windows is the most popular OS for user workstations, you are very likely to handle user-driven Windows Initial Access alerts frequently. The methods are covered by these MITRE techniques:

- T1566 / Phishing: Threat actors employ different phishing techniques, tricking users into launching the malware themselves
- T1091 / Removable Media: Threat actors infect a USB device and hope that users will use the USB on multiple PCs

# Initial Access via RDP

1) **Network Scan Botnet scans our IP and detects an exposed RDP port**: N/A. Network attacks are out of the room scope
2) **RDP Brute Force Botnet starts a brute force of common user names (Administrator, admin, support, etc.)**: 
    - Open Security logs and filter for the failed logins (event ID 4625)
    - Filter for logon types 3 and 10, meaning remote logons
    - Filter for logins from external IPs (use "Source IP" field)
    - That's it. You have detected a potential RDP brute force
3) **Initial Access via RDP After around 100 attempts, the botnet guesses the correct password and enters the system**:
    - Continue with the list from the previous step
    - Switch the event ID filter to 4624 (successful logins)
    - Check the account under which the logon was made
    - Now you know which account was used for the Initial Access
4) **Further Malicious Actions Two hours after the breach, the threat actor logs in via RDP and reviews the Desktop**:
    - Continue with the list from the previous step
    - Filter for logon type 10, indicating interactive RDP login
    - Copy the "Logon ID" field from the logon event
    - Open Sysmon logs and search events with the same "Logon ID"
    - You will see all processes started by the threat actor via RDP

# Initial Access via Phishing - Binary Attachements

In Windows, there are lots(opens in new tab) of executable extensions, and while most people know not to open untrusted .exe files, they are usually less cautious about .com, .scr, or .cpl files. However, all of them can contain the malware inside. For example, users are very likely to open the attached "tryhatme.com" file name assuming it to be a link to a meeting invite, not a malicious binary.

To make it worse, Windows hides known file extensions by default, meaning that the file "program.exe" will be shown to you just as "program". Threat actors often abuse it by naming their viruses like "invoice.pdf.exe" or "cat.png.com" and changing the icons to fit the topic.

## LNK Attachments

To avoid AV detection, threat actors may prefer attaching PowerShell, Visual Basic, or BAT scripts over binaries. A popular way to make the scripts look trustworthy is to hide them behind LNK shortcuts - the same files you have on your Desktop that point to real executables somewhere in the Program Files folder.
Unlike with binary attachments, LNK files have a very interesting and important capability - they leave little execution trace.

## Detecting Malicious Download

It is relatively simple to hunt for malicious downloads if you know how the victim sees it. First, the user uses a web browser or desktop application to open a phishing attachment. In the simplest case, it would be a direct .exe malware download, but you are far more likely to see an archive attachment like .zip or .rar containing the malware. In this case, Sysmon can greatly help you detect every attack stage.

# Initial Access via USB

Initial Access via an infected USB bypasses firewalls, much like phishing, and can start the attack chain even without Internet access and continue spreading without user interaction.

## Defecting an Infected USB

- Malware hides all legitimate files on USB and creates a malicious "RECOVERY.lnk" file
- Malware creates a "Photos.exe" binary and sets its icon to look like a simple folder
- Malware creates double-extension copies of all files, like "photo_2024_1_12.jpg.exe"