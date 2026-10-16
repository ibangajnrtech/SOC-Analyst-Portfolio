# Introduction to Computer Forensics

This involves gathering evidence of activities performed on computers.
It deals with forensic analysis of all types of digital devices, including recovering, examining, and analysing data found in digital devices. 

## Computer Forensic Artifacts

They are essential pieces of information that provide evidence of human activity in a computer.

# Windows Registry and Foresnics

The windows registry is a collection of databases that contains the system's configuration data. This configuration data can be about the hardware, software or the user's information.

## Structure of the Registry:

1) **HKEY_CURRENT_USER:** Contains the root of the configuration information for the user who is currently logged on. The user's folders, screen colors, and Control Panel settings are stored here. This information is associated with the user's profile. This key is sometimes abbreviated as HKCU.
2) **HKEY_USERS:** Contains all the actively loaded user profiles on the computer. HKEY_CURRENT_USER is a subkey of HKEY_USERS. HKEY_USERS is sometimes abbreviated as HKU.
3) **HKEY_LOCAL_MACHINE:** Contains configuration information particular to the computer (for any user). This key is sometimes abbreviated as HKLM.
4) **HKEY_CLASSES_ROOT:** Is a subkey of HKEY_LOCAL_MACHINE\Software . The information that is stored here makes sure that the correct program opens when you open a file by using Windows Explorer. This key is sometimes abbreviated as HKCR.
5) **HKEY_CURRENT_CONFIG:** Contains information about the hardware profile that is used by the local computer at system startup.

# Accessing registry Hives Offline

The majority of these hives are located in the `C:\Windows\System32\Config` directory and are:

1) **DEFAULT** (mounted on `HKEY_USERS\DEFAULT`)
2) **SAM** (mounted on `HKEY_LOCAL_MACHINE\SAM`)
3) **SECURITY** (mounted on `HKEY_LOCAL_MACHINE\Security`)
4) **SOFTWARE** (mounted on `HKEY_LOCAL_MACHINE\Software`)
5) **SYSTEM** (mounted on `HKEY_LOCAL_MACHINE\System`)

## Hives containing user information:

Apart from these hives, two other hives containing user information can be found in the User profile directory. For Windows 7 and above, a user’s profile directory is located in `C:\Users\<username>\` where the hives are:

1) **NTUSER.DAT** (mounted on HKEY_CURRENT_USER when a user logs in): The NTUSER.DAT hive is located in the directory `C:\Users\<username>\`.
2) **USRCLASS.DAT** (mounted on HKEY_CURRENT_USER\Software\CLASSES): The USRCLASS.DAT hive is located in the directory `C:\Users\<username>\AppData\Local\Microsoft\Windows`.

## The Amcache Hive:

This hive is located in `C:\Windows\AppCompat\Programs\Amcache.hve`. Windows creates this hive to save information on programs that were recently run on the system.

# Data Acquisition

When performing forensics, we will either encounter a live system or an image taken of the system. For the sake of accuracy, it is recommended practice to image the system or make a copy of the required data and perform forensics on it.
The forensically correct method is to acquire a copy of this data and perform analysis on that. 

For acquiring these files, we can use one of the following tools:

1) **Kape**: KAPE is a live data acquisition and analysis tool which can be used to acquire registry data.
2) **Autopsy**: Autopsy gives you the option to acquire data from both live systems or from a disk image.
3) **FTK Imager**: FTK Imager is similar to Autopsy and allows you to extract files from a disk image or a live system by mounting the said disk image or drive in FTK Imager.

# Exploring Windows Registry

Once we have extracted the registry hives, we need a tool to view these files as we would in the registry editor. Since the registry editor only works with live systems and can't load exported hives, we can use the following tools:

1) **Registry Viewer**: AccessData's Registry Viewer has a similar user interface to the Windows Registry Editor. There are a couple of limitations, though. It only loads one hive at a time, and it can't take the transaction logs into account.
2) **Zimmerman's Registry Explorer**:  It can load multiple hives simultaneously and add data from transaction logs into the hive to make a more 'cleaner' hive with more up-to-date data. It also has a handy 'Bookmarks' option containing forensically important registry keys often sought by forensics investigators. Investigators can go straight to the interesting registry keys and values with the bookmarks menu item. 
3) **RegRipper**: RegRipper is a utility that takes a registry hive as input and outputs a report that extracts data from some of the forensically important keys and values in that hive. The output report is in a text file and shows all the results in sequential order. It does not take the transaction logs into account. We must use Registry Explorer to merge transaction logs with the respective registry hives before sending the output to RegRipper for a more accurate result.

# System Information and System Accounts

When we start performing forensic analysis, the first step is to find out about the system information.

## OS Version:

If we only have triage data to perform forensics, we can determine the OS version from which this data was pulled through the registry. To find the OS version, we can use the following registry key: `SOFTWARE\Microsoft\Windows NT\CurrentVersion`

## Current Control Set:

The hives containing the machine’s configuration data used for controlling system startup are called Control Sets. Commonly, we will see two Control Sets, ControlSet001 and ControlSet002, in the SYSTEM hive on a machine. In most cases (but not always), ControlSet001 will point to the Control Set that the machine booted with, and ControlSet002 will be the last-known-good  configuration. Their locations will be: `SYSTEM\ControlSet001`, `SYSTEM\ControlSet002`

## Computer Name:

It is crucial to establish the Computer Name while performing forensic analysis to ensure that we are working on the machine we are supposed to work on. We can find the Computer Name from the following location: `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`

## Time Zone Information:

For accuracy, it is important to establish what time zone the computer is located in. This will help us understand the chronology of the events as they happened. For finding the Time Zone Information, we can look at the following location: `SYSTEM\CurrentControlSet\Control\TimeZoneInformation`

## Network Interfaces and Past Networks:

The following registry key will give a list of network interfaces on the machine we are investigating: `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`

The past networks a given machine was connected to can be found in the following locations: `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged` and `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed`

## Autostart Programs(Autoruns):

The following registry keys include information about programs or commands that run when a user logs on. 
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`
`SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
`SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`
`SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

## SAM hive and User Information:

The SAM hive contains user account information, login information, and group information. This information is mainly located in the following location: `SAM\Domains\Account\Users`

# Usage or Knowledge of files/folders

Windows maintains a list of recently opened files for each user. This information is stored in the NTUSER hive and can be found on the following location: `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`

Another interesting piece of information in this registry key is that there are different keys with file extensions, such as  .pdf , .jpg , .docx  etc. These keys provide us with information about the last used files of a specific file extension. So if we are looking specifically for the last used PDF files, we can look at the following registry key: `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.pdf`

## Office Recent Files:

Similar to the Recent Docs maintained by Windows Explorer, Microsoft Office also maintains a list of recently opened documents. This list is also located in the NTUSER hive. It can be found in the following location: `NTUSER.DAT\Software\Microsoft\Office\VERSION`
The version number for each Microsoft Office release is different. An example registry key will look like this: `NTUSER.DAT\Software\Microsoft\Office\15.0\Word`

## ShellBags:

When any user opens a folder, it opens in a specific layout. Users can change this layout according to their preferences. These layouts can be different for different folders. This information about the Windows 'shell'  is stored and can identify the Most Recently Used files and folders. Since this setting is different for each user, it is located in the user hives. We can find this information on the following locations: `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`, `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`, `NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU`, `NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags`
Registry Explorer doesn't give us much information about ShellBags. However, another tool from Eric Zimmerman's tools called the `ShellBag Explorer` shows us the information in an easy-to-use format. 

## Open/Save and LastVisited Dialog MRUs:

When we open or save a file, a dialog box appears asking us where to save or open that file from. It might be noticed that once we open/save a file at a specific location, Windows remembers that location. This implies that we can find out recently used files if we get our hands on this information. We can do so by examining the following registry keys:
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU`
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU`

## Windows Explorer Address/Search Bar:

Another way to identify a user's recent activity is by looking at the paths typed in the Windows Explorer address bar or searches performed using the following registry keys, respectively.
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths`
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery`

# Evidence of Execution

## UserAssist:

Windows keeps track of applications launched by the user using Windows Explorer for statistical purposes in the User Assist registry keys. These keys contain information about the programs launched, the time of their launch, and the number of times they were executed. However, programs that were run using the command line can't be found in the User Assist keys. The User Assist key is present in the NTUSER hive, mapped to each user's GUID. We can find it at the following location:
`NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`

## ShimCache:

ShimCache is a mechanism used to keep track of application compatibility with the OS and tracks all applications launched on the machine. Its main purpose in Windows is to ensure backward compatibility of applications. It is also called Application Compatibility Cache (AppCompatCache). It is located in the following location in the SYSTEM hive:
`SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`
ShimCache stores file name, file size, and last modified time of the executables.
Registry Explorer, doesn't parse ShimCache data in a human-readable format, so we go to another tool called `AppCompatCache Parser`.
We can use the following command to run the AppCompatCache Parser Utility:
`AppCompatCacheParser.exe --csv <path to save output> -f <path to SYSTEM hive for data parsing> -c <control set to parse>`

The output can be viewed using `EZviewer`, another one of Eric Zimmerman's tools.

## AmCache:

The AmCache hive is an artifact related to ShimCache. This performs a similar function to ShimCache, and stores additional data related to program executions. This data includes execution path, installation, execution and deletion times, and SHA1 hashes of the executed programs. This hive is located in the file system at:
`C:\Windows\appcompat\Programs\Amcache.hve`

Information about the last executed programs can be found at the following location in the hive:
`Amcache.hve\Root\File\{Volume GUID}\`

## BAM/DAM:

Background Activity Monitor or BAM keeps a tab on the activity of background applications. Similar Desktop Activity Moderator or DAM is a part of Microsoft Windows that optimizes the power consumption of the device.

In the Windows registry, the following locations contain information related to BAM and DAM. This location contains information about last run programs, their full paths, and last execution time.
`SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}`
`SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}`

# External Devices/USB Device Forensics

When performing forensics on a machine, often the need arises to identify if any USB or removable drives were attached to the machine. If so, any information related to those devices is important for a forensic investigator.

## Device Identification:

The following locations keep track of USB keys plugged into a system. These locations store the vendor id, product id, and version of the USB device plugged in and can be used to identify unique devices. These locations also store the time the devices were plugged into the system.
`SYSTEM\CurrentControlSet\Enum\USBSTOR`
`SYSTEM\CurrentControlSet\Enum\USB`

## First/Last Times:

 the following registry key tracks the first time the device was connected, the last time it was connected and the last time the device was removed from the system.

SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####

In this key, the #### sign can be replaced by the following digits to get the required information:

1) **0064**: First Connection time
2) **0066**: Last Connection time
3) **0067**: Last removal time

## USB device Volume Name:

The device name of the connected drive can be found at the following location:
`SOFTWARE\Microsoft\Windows Portable Devices\Devices`