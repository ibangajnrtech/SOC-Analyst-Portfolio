# Filenames and Paths

Attackers may use different disk locations to hide their actions and reduce visibility. For example:

- `C:\` (System drive) can be a common target for persistence mechanisms.
- `C:\Users\Public` profile can enable cross-user access of detonated adversary tools.
- `C:\Users\Public\Public Downloads` provides a high-traffic directory that would often evade strict monitoring.

Additionally, adversaries may utilise other malware staging patterns such as:

- Utilising temporary directories such as `C:\Windows\Temp\` for ephemeral payloads.
- Placing payloads in writable system paths, such as `C:\ProgramData\` for stealth persistence.

## Filename Heuristic Indicators

Attackers are also known to modify filenames to escape detection through implementing various types of heuristic indicators, including:

- Double extensions - An example is `invoice.pdf.exe`, which leverages Windows' default settings that hide file extensions.

- System binary impersonation - A filename such as scvhost.exe abuses the user's familiarity with core system processes. Defenders should include legitimate locations for system processes in an allowlist, rather than standalone filenames.

- High-entropy Strings – A filename such as `jh8F21.exe` suggests automated packing or polymorphic generation, which is commonly used in a high-churn phishing operation.

- Masquerading - Filenames such as `backup-2300.exe` can blend with routine files, thus leveraging on reduced suspicion. Another example is a single character substitution, which can bypass detection while looking visually legitimate to an unsuspecting employee.

# File Hash Lookup

It is advisable to work with potentially malicious binaries in a separate, isolated environment.
We can generate and verify our file hash using the following commands:

- **Command prompt**: `certutil -hashfile bl0gger.exe SHA256`
- **PowerShell**: `Get-Filehash -Algorithm SHA256 bl0gger.exe`
- **Linux**: `sha256sum bl0gger.exe`

A few pointers as analysts when dealing with hashes:

- Store the hashes in lowercase to avoid needless differences.
- Hash what matters in your investigation. For example, if the malware resides in a ZIP file, hash both the archive and the extracted binary.
- Do not leave plain strings without the context of where and when you encountered them.
- Any byte change will change the resulting hash.

## Analysis with VirusTotal

There are several items from the search results that would be worth taking note of when you submit a hash:

- **Detection score**: This represents a crowdsourced security verdict from various vendors displayed as a ratio. The higher the number, the higher the confidence threat.
- **Threat labels and categories**: These are vendor-specific classifications of threats that help confirm their attribution across vendors.
- **Detection rules**: These are the technical signatures used by AV engines to identify threats. Typical classifications are YARA rules, Heuristic patterns, and behavioural triggers.
- **Properties**: This is where the core metadata for the file is stored, including the file type, size, and compilation timestamp.
- **Contained domains and IPs**: This information covers the malware's network infrastructure.
- **Contained files**: This section details any files embedded or dropped during the malware's execution.

## Cross-Reference with MalwareBazaar

MalwareBazaar(opens in new tab) is an all-in-one database for malware collection and analysis. The project supports the following features:

- **Malware Samples Upload**: Security analysts can upload their malware samples for analysis and build the intelligence database. This can be done through the browser or an API.
- **Malware Hunting**: Hunting for malware samples is possible through various elements, such as :
    - **Malware Family tagging**: You will find files classified by their malware families. An example of this is a file with only 5/70 detections on VirusTotal but tagged as #IcedID in MalwareBazaar; it should be treated as malicious.
    - **YARA rule integration**: Many submissions will include rules that detect related samples. As an analyst, you should take note of these rules to be added to the EDR/SIEM for future hunting.
    - **Campaign attribution**: Tags such as #TA551, which belong to a threat actor group, help link observed incidents to known adversaries. This can help identify coordinated attacks against an environment.
    - **Sample Availability**: Malware samples are available for download and analysis. Reanalysing samples in a sandbox is best practice, which we shall cover in the next task.

# Sandbox Analysis 

Static properties such as hashes, strings, and sections tell you identity. To understand the impact of malicious files, you need to execute them safely in a controlled environment. Sandboxes provide a disposable VM instrumented to capture every process, registry write, and network packet associated with the malware being investigated.

Security analysts will use sandboxes to do three things:

- Confirm execution - if nothing happens, your alert might be a decoy.
- Extract runtime IOCs - domains, mutexes, dropped payloads, feed hunts and detections.
- Map to ATT&CK - most sandboxes auto‑label behaviour with technique IDs, fast‑tracking your report.

## Sandboxing Tools

The most widely used tools for detonating wild malware are Hybrid Analysis(opens in new tab) and Joe Sandbox(opens in new tab), each with its distinct application:

- Hybrid Analysis (HA) focuses on behaviour trees and a clean MITRE ATT&CK heatmap. It is suitable for a fast executive summary.
- Joe Sandbox (JS): goes deep, covering system calls, strings, and memory dumps. Great for reverse engineers and detection engineers.

## Sandboxing Limitations

These limitations include, but are not limited to, the following:

1) **Sandbox Evasion Techniques**: Threat actors would design their payloads to detect and evade sandboxes, leading to false negatives. Some of the common evasion tactics used include:
    - Environment Awareness Checks: Malware checks for signs of virtualised/sandboxed environments
    - Anti-Debugging & Anti-Sandboxing Tricks: Malware conducts debugger detection and checks for unique hardware IDs.
2) **Limited Execution Time & Coverage**: Most sandboxes terminate analysis after 2-5 minutes, which means multi-stage malware may not fully execute. Additionally, time-delayed attacks will evade detection. As previously covered, this would mean cross-referencing other threat intelligence resources.

3) **Encrypted & Obfuscated Traffic**: Many sandboxes cannot decrypt SSL/TLS traffic, leading to blind spots. This may result in HTTPS C2 Traffic appearing with no payload visibility or the malware utilising DNS tunnelling to hide data in DNS queries.

4) **Fileless & Living-off-the-Land (LotL) Malware**: Some threats never touch disk, bypassing traditional sandbox analysis by employing PowerShell Attacks and WMI Persistence.

