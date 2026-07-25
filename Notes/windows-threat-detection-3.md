# Simplest C2

For other Initial Access methods, threat actors can't simply use RDP every time they need to run a command, so they need some process that connects back to the attackers and waits for their commands 24/7.

# Persistence Overview

Data stealer infections usually have a very short lifespan: they breach the victim, collect the data, exfiltrate it, and exit - all within minutes. However, for most other attacks, maintaining access to the victim for days or even months after the Initial Access is vital. The tactic of maintaining reliable, long-term access to the target that can survive reboots and password changes is called Persistence.

## Persisting via RDP

Many Windows breaches happen because of the exposed service: RDP with a weak password, a vulnerable mail server, or a misconfigured web app. For such scenarios, the threat actors can access the machine via the same exposed service over and over again until the vulnerability is fixed. Still, threat actors often deploy an additional Persistence method, for example:

- Create an additional hidden vulnerability in the breached service (e.g. a backdoor or a web shell)
- Create a new user (T1136), make it an administrator (T1098), and use it for further RDP logins

Focusing on the second method (Create a new user (T1136), make it an administrator (T1098)), we can see how you or threat actors can manage users on Windows. The first option is to use the graphical utility by searching for "Computer Management" or by launching lusrmgr.msc. The second option is to use a command line like this:

# 1. Two methods to create the "mr.backd00r" user
- CMD C:\> `net user "mr.backd00r" "p@ssw0rd!" /add`
- PS  C:\> `New-LocalUser "mr.backd00r" -Password [...]`

# 2. Two methods to add the user to Administrators 
- CMD C:\> `net localgroup Administrators "mr.backd00r" /add`
- PS  C:\> `Add-LocalGroupMember "Administrators" -Member "mr.backd00r"`

## Detecting Backdoored Users

Every user creation event is logged as Security event ID 4720.
Since threat actors can be very creative with naming the backdoored accounts, you should not rely just on detecting suspicious names like "hacker" but rather investigate:

1) Who created the account? Can the person confirm the account creation?
2) What is the source IP and time of the creator's login? Is it expected?
3) Which other suspicious events can you see in the creator's session?

### Making Users Privileged

Next, a new user by itself won't give the attacker much, as the default user permissions do not allow remote (RDP) logins or grant administrative privileges on the machine. To overcome this, threat actors will add their backdoored account to one of the privileged groups, which is tracked by Security event ID 4732. The most commonly exploited groups are Administrators and Remote Desktop Users.

### Resetting Passwords

Lastly, in more advanced cases, threat actors may simply reset the password of some old or unused account and use it instead of creating a new one. You can detect it with Security event ID 4724.

# Persistence: Tasks and Services

Persistence via a backdoored user works well if you can remotely log in to it via RDP, but if the attack started through a phishing attack or USB infection, that's not an option. For these scenarios, threat actors need an actively running malware that maintains a connection with their C2 server even after a system reboot. How could they achieve malware persistence?

## Services and Tasks

1) **Create a Windows Service (Runs after OS startup)**: `sc create "BadService" binpath= "C:\malware.exe" start= auto`
    - **Launch of sc.exe**: Sysmon / 1
    - **Service Creation**: Security/4697

2) **Create a Scheduled Task (Run after OS startup)**: `schtasks /create /tn "BadTask" /tr "C:\malware.exe" /sc onstart /ru System`
    - **Launch of schtasks.exe**: Sysmon/1
    - **Scheduled task Creation**:  Security/4698

## Detecting Services

You can view services by launching services.msc or searching for "Services", but you need administrative privileges and the sc.exe command to create or modify one.

In logs, you can detect malicious services in three ways:

1) Detect the launch of the sc.exe create command via Sysmon event ID 1
2) Detect service creation via Security event ID 4697 or System event ID 7045
3) Detect suspicious processes with a services.exe parent process

## Detecting Tasks

Scheduled tasks are another Windows feature heavily used by both the OS and external apps (e.g. to check for updates or make a backup every hour). From GUI, you can manage tasks by launching taskschd.msc or searching for "Task Scheduler". From the command line, you can use the schtasks.exe command.
Unlike services, scheduled tasks are very easy to configure and hide, which is why they are the most common persistence method by threat actors, like in these APT28(opens in new tab) and APT41(opens in new tab) attacks. Similar to services, you can detect scheduled tasks in three ways:

1) Detect the launch of the `schtasks.exe /create` command via Sysmon event ID 1
2) Detect and analyze scheduled task creation events via Security event ID 4698
3) Detect suspicious processes with a `svchost.exe [...] -s Schedule` parent

# Persistence: Run Keys and Startup

Services and scheduled tasks are typically run on system boot and require administrative privileges to configure. However, what if a program has to run only when a specific user logs in? For such cases, Windows provides a few per-user persistence methods that are actively used by both legitimate tools and malware:

1) **Add malware to Startup Folder (Runs upon user login)**: `copy C:\malware.exe "%AppData%\Microsoft\Windows\Start Menu\Programs\Startup\malware.exe"`
    - **New startup item**: Sysmon Event ID 11
2) **Add malware to "Run" keys (Runs upon user login)**: `reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v BadKey /t REG_SZ /d "C:\malware.exe"`
    - **New registry value**: Sysmon Event ID 13

## Detecting Startup

The startup folder was meant to be an easy way for inexperienced users to configure programs to run on login. You simply open the startup folder, move your program or program shortcut there, and see how it automatically starts upon your future logins. You can access your startup folder via the path below:
- `C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\`
- For all Users: `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`
The startup folder is not a common choice for legitimate programs, so usually, the folder is empty. Still, threat actors often put their malware there, and you can detect it by monitoring file creation events (Sysmon Event ID 11) inside the Startup Folder. Also, note that the programs launched via startup will have an explorer.exe parent, so it may be hard to differentiate them from legitimate user activity or attacks.

## Detecting Run Keys

Run key persistence is very similar to the startup folder; they even share a single MITRE technique! The only major difference is how the entries are added there. Instead of just copying the program to the startup folder, you need to create a new value in the "Run" Windows registry and put the path to your program there:
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
- For all Users: `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`

# Need for Persistence

1) **Add the host to a botnet and use it for further attacks**
- Like how the Kraken Botnet combines crypto miner, data stealer, and C2 capabilities

2) **Spy on the victim as a part of a state-sponsored campaign**
- Like how Volt Typhoon stayed undetected in the US electric grid for nearly a year

3) **Use the victim as an entry point to the network, breaching which could take months**
- Like in the case where threat actors spent 29 days breaching a full network
