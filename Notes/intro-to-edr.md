# What is an EDR?

Endpoint Detection and Response (EDR) is a security solution designed to monitor, detect and respond to advanced threats at the endpoint level.
Examples of EDR solutions in the market are:
- CrowdStrike Falcon
- SentinelOne ActiveEDR
- Microsoft Defender for Endpoint
- OpenEDR
- Symantec EDR

## Features of EDR

1. **Visibility**: It collects detailed data from the endpoints, which includes process modifications, registry modifications, file and folder modifications, user actions, and much more. It then presents this information in a very structured format to the analyst.

2. **Detection**: It incorporates signature-based detections as well as behavious-based detections, such as unexpected user activities. With modern machine learning capabilities, it identifies any deviation from the baseline behaviour and instantly flags it.

3. **Response**: EDR also empowers analysts to take action on detected threats. These actions can be taken at any endpoint within the central EDR console. As an analyst, you may decided to isolate a complete endpoint, terminate a process, or quarantine some files.

# EDR Telemetry

Telemetry is the black box of an endpoint with everything necessary for detection and investigation.

## Collected Telemetry

It is often difficult to differentiate between regular and malicious activities in the endpoints. The more data is collected, the better judgements can be made. Examples of Telemetry that it collects are:
1. **Process Executions and Terminations**: It keeps track of all the running and idle processes, which helps to identify suspicious child-parent process relationships, suspicious executables initiating a process, malware payloads, etc.
2. **Network Connections**: All the endpoints network connections are monitored, which helps identify any connection to a C2 server, unusual port usage, data exfiltration, or lateral movement within the network.
3. **Command Line Activity**: It captures all the commands executed on the endpoints in CMD, Powershell, etc, which helps to identify malicious command execution, obfuscated Powershell script executions, which are often missed by a traditional antivirus.
4. **Files and Folders Modifications**: Threat actors modify different files and folders during data stagingm ransomware executions, and malicious file dropping. The EDR tracks this.
5. **Registry Modifications**: The registry is a goldmine of informaiton about the configurations in Windows system. There are many registry modifications that occue during a malicious activity, and most of these are monitores by the EDR.

# Detection

Based on telemetry received from the endpoints, some advanced detection techniques are applied to this data. Example:
1. **Behavioural Detection**: Instead of just matching the signatures with known threats, it observes the complete behaviour of a file. Advanced threats craft their malware to look clean and use legitimate processes to carry out their attack. EDR catches this behaviour.
>**Example**: A process winword.exe spawning powershell.exe will be flagged by EDR due to the behaviour. A word document spawning a powershell is an unusual parent-child relationship

2. **Anomaly Detection**: With time, EDR understands the baseline behaviour of the endpoints. Any activity that deviates from this behaviour will be flagged. During any malicious activity, the endpoints behaviour deviates from normal. EDR picks it up. Sometimes, this can generate false positives as well.
>**Example**: On one of the endpoints, a process modifies an auto-start registry key, which is not a common behaviour on the endpoint.

3. **IOC matching**: EDRs have some strong threat intelligence field integrations. Except for zero-day attacks, most of the attacks have indicators published in the threat intelligence feeds. EDR flags any activity that matches any known IOC.
>**Example**: A user downloads a file that drops an executable. The executable is often used in a specific attack. The hash of this executable will get matched with the threat intelligence feed and instantly flagged by the EDR.

4. **MITRE ATT&CK Mapping**: Any activity flagged by the EDR is not only marked as malicious or suspicious but also mapped with the MITRE Tactic and Technique (attack stage) that the particular activity was on.
>**Example**: If the EDR flags the creation of a scheduled task for any reason, it will likely map this activity to the following:
>- Tactic: Persistence
>- Technique: Scheduled Task/Job

5. **Machine Leaning Algorithms**: Advanced threat actors try to evade defenses as much as possible, and their activities may sometimes bypass advanced detection techniques. Modern EDRs have machine learning models trained by a large dataset of normal and malicious behaviours. This can detect complex patterns of an attack.
>**Example**: Attacks in which the individual actions are not inherently malicious, but the ML algorithm identifies the whole chain of activities as malicious. Fileless attacks and multistaged intrusions are often detected through this.

# Response

The next step after any detection is the response. EDR offers both automated and manual response. Policies can be made to block known malicious behaviours automatically.

1. **Isolate Host**: During any malicious activity on an endpoint, you can isolate that endpoint from the network through EDR. This is a very effective function for containing malicious activity. Most attacks start from a single endpoint and move laterally to other endpoints to compromise the whole network. Isolating the infected endpoint on time can stop this from happening.

2. **Terminate Process**: Not every malicious activity requires host isolation. Some hosts run the core business operations, and isolating them can cause more loss than the malicious activity. Terminating a process is enough to neutralize the malicious activity. This action should be taken consciously since terminating a legitimate process can disrupt the endpoint.

3. **Quarantine**: If a malicious file comes into the endpoint, it can be quarantined. Quarantine ensures that the file is moved to an isolated location where it can not be executed. The analysts can then review the file to restore or permanently remove it.

4. **Remote Access**: Analysts can also remotely access the shell of any endpoint. This is often done when the EDR's built-in repsonse is not enough to take action on a specific activity. Through remote access, analysts can gain deeper visibility  into the system or take custom actions within the endpoints. Analysts can also run scripts of collect their desired data from the host through remote access.

5. **Artefacts Collection**: The analysts may need to extract some data from the endpoints for detailed forensice investigation or reporting for legal actions. Most commonly extracted artefacts include:
- Memory Dump
- Event logs
- Specific Folder Contents
- Registry Hives

