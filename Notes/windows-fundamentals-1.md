# The File System

The file system used in modern versions of Windows is the New Technology File System or simply NTFS.
Before NTFS, there was  FAT16/FAT32 (File Allocation Table) and HPFS (High Performance File System).
NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.

**NTFS**:
- Supports files larger than 4GB
- Sets specific permissions on folders and files
- Folder and file compression
- Encryption (Encryption File System or EFS)

On NTFS volumes, you can set permissions that grant or deny access to files and folders.
The permissions are:
- Full control
- Modify
- Read & Execute
- List folder contents
- Read
- Write

How can you view the permissions for a file or folder?
1) Right-click the file or folder you want to check for permissions.
2) From the context menu, select Properties.
3) Within Properties, click on the Security tab.
4) In the Group or user names list, select the user, computer, or group whose permissions you want to view.

Another feature of NTFS is Alternate Data Streams ( ADS ).
Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System).

# The Windows/System32 Folders

The Windows folder (C:\Windows) is traditionally known as the folder which contains the Windows operating system.
The System32 folder holds the important files that are critical for the operating system.

# User Accounts, Profiles and Permissions

User accounts can be one of two types on a typical local Windows system: Administrator & Standard User.
Each user profile will have the same folders; a few of them are:
- Desktop
- Documents
- Downloads
- Music
- Pictures
Another way to access this information, and then some, is using Local User and Group Management.
Right-click on the Start Menu and click Run. Type `lusrmgr.msc`.

