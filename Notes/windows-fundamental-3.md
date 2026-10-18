# Windows Update

Windows Update is a service provided by Microsoft to provide security updates, feature enhancements, and patches for the Windows operating system and other Microsoft products, such as Microsoft Defender. 
Windows Update is located in Settings.
Another way to access Windows Update is from the Run dialog box, or CMD, by running the command `control /name Microsoft.WindowsUpdate`.

# Windows Security

**Windows Security** is your home to manage the tools that protect your device and your data.

# Virus & Threat Protection

Virus & threat protection is divided into two parts:
- Current threats
- Virus & threat protection settings

# Firewall and Network Protection

Windows Firewall offers three firewall profiles: domain, private and public
- **Domain** - The domain profile applies to networks where the host system can authenticate to a domain controller. 
- **Private** - The private profile is a user-assigned profile and is used to designate private or home networks.
- **Public** - The default profile is the public profile, used to designate public networks such as Wi-Fi hotspots at coffee shops, airports, and other locations.
Command to open the Windows Defender Firewall is `WF.msc`.

# App & Browser control

You can change the settings for the **Microsoft Defender SmartScreen**.
Microsoft Defender SmartScreen protects against phishing or malware websites and applications, and the downloading of potentially malicious files.

# BitLocker

BitLocker Drive Encryption is a data protection feature that integrates with the operating system and addresses the threats of data theft or exposure from lost, stolen, or inappropriately decommissioned computers.
On devices with TPM installed, BitLocker offers the best protection.

# Volume Shadow Copy Service

The **Volume Shadow Copy Service** (VSS) coordinates the required actions to create a consistent shadow copy (also known as a snapshot or a point-in-time copy) of the data that is to be backed up.
Volume Shadow Copies are stored on the System Volume Information folder on each drive that has protection enabled.
If VSS is enabled (System Protection turned on), you can perform the following tasks from within advanced system settings. 
- Create a restore point
- Perform system restore
- Configure restore settings
- Delete restore points