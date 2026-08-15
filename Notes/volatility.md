# Volatility Overview

Volatility is a free memory forensics tool commonly used by malware and SOC analysts within a blue team or as part of their detection and monitoring solutions.

## Analyzing a dump

1) We must first identify the image type
2) Then analyse using various volatility plugins against it

# Memory Extraction

First we need to extract the memory dump from the machine based on the requirements of our investigation. 
Some of the techniques and tools that can be used to extract a memory from a bare-metal machine:

1) FTK Imager
2) Redline
3) Dumplt.exe
4) win32dd.exe / win64dd.exe
5) Memoryze
6) FastDump

**N/B**: When using an extraction tool on a bare metal host, it can usually take a considerable amount of time.

For lab machines, gathering a memory file can easily be done by collecting the virtual memory file from the host machine's drive.
This file can change depending on the hypervisor used; som examples of hypervisor virtual memory files:
- VMWare - `.vmem`
- Hyper-V - `.bin`
- Parallels - `.mem`
- VirtualBox - `.sav` file (this is only a partial memory file)

# Identifying Image Info and Profiles

Using the `imageinfo` plugin, you can find the exact version and build the machine you extracted a memory dump from was.
If we are still looking to get information about what the host is running from the memory dump, we can use the following three plugins `windows.info`, `linux.info`, `mac.info`. This plugin will provide information about the host from the memory dump.

Syntax: `python3 vol.py -f <file> windows.info`

# Listing Processes and Connections

1) The most basic way of listing is using `pslist` which will get the list of processes from the doubly-linked list that keeps track if processes in memory.
Syntax: `python3 vol.py -f <file> windows.pslist`

2) Some malware, typically rootkits will in an attempt to hide their processes, unlink itself from the list. To combat this evasion technique, we can use `psscan`; this technique of listing processes will locate processes by finding data structures that match `_EPROCESS`. It could also cause false positives.
Syntax: `python3 vol.py -f <file> windows.psscan`

3) `pstree` does not offer any kind of special techniques to help identify evasion like the last two plugins. This plugin lists all processes based on their parent process ID, using the same method as `pslist`.
Syntax: `python3 vol.py -f <file> windows.pstree`

4) `netstat` will attempt to identify all memory structures with a network connection.
Syntax: `python3 vol.py -f <file> windows.netstat`

5) `dlllist` will list all DLLs associated with processes at the time of extraction.
Syntax: `python3 vol.py -f <file> windows.dlllist`

You can utilise other tools like bulk_Extractor to extract a PCAP file from the memory file.

# Volatility Hunting and Detection Capabilities

1) `malfind` which is mostly useful when hunting for code injection. This plugin will attempt to identify injected processes and their PIDs along with the offset address and a Hex, Ascii, and Disassembly view of the infected area.
Syntax: `python3 vol.py -f <file> windows.malfind`

2) `yarascan` will searching for strings, patterns and compound rules against a rule set. 
Syntax: `python3 -f <file> windows.yarascan`

# Advanced Memory Forensics

There are five methods of hooking employed by adversaries:
- SSDT Hooks
- IRP Hooks
- IAT Hooks
- EAT Hooks
- Inline Hooks

1) `ssdt` plugin will seach for hooking and output its results. SSDT stands for System Service Descriptos Table which the windows kernel users to look up system functions. An adversary can hook into this table and modify pointers to point to a location the rootkit controls.
Use this plugin after investigating the initial compromise and working off it as part of your lead investigation.
Syntax: `python3 vol.py -f <file> windows.ssdt`

2) `modules` plugin will dump a list of loaded kernel modules which can be useful in identifying active malware. If a malicious file is idly waiting or hidden, this plugin may miss it.
This plugin is best used once you have further investigated and found potential indicators to use as input for searching and filtering.
Syntax: `python3 vol.py -f <file> windows.modules`

3) `driverscan` plugin will scan for drivers present on the system at the time of extraction which can help to identify driver files in the kernel that the `modules` plugin might have missed or were hidden.
As with the last plugin, it is again recommended to have a prior investigation before moving on to this plugin. It is also recommended to look through the `modules` plugin before `driverscan`.
Syntax: `python3 vol.py -f <file> windows.driverscan`.

Other plugins helpful when attempting to hunt for advanced malware in memory:
- modscan
- driverirp
- callbacks
- idt
- apihooks
- moddump
- handles