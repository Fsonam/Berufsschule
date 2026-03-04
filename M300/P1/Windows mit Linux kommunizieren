# Linux → Windows SMB Connection

## Goal
The goal of this exercise is to allow a Linux client to access a shared folder on a Windows server using the SMB protocol.

---

# 1. Preparation

## Start the Virtual Machines

Start the following VMs:

- Windows VM (Server)
- Linux VM (Client)

---

## Check IP Addresses

### Windows

Open the command prompt and run:

```
ipconfig
```

Example output:

```
192.168.210.132
```

---

### Linux

Open a terminal and run:

```
ip a
```

Example output:

```
192.168.210.134
```

---

## Test Network Connectivity

From Linux:

```
ping WINDOWS_IP
```

Example:

```
ping 192.168.210.132
```

If the ping works, the network connection is correct.

---

# 2. Windows Configuration

## Create a Folder

Create the following folder on Windows:

```
C:\data\downloads
```

---

## Share the Folder

1. Right-click the folder **downloads**
2. Open **Properties**
3. Go to **Sharing**
4. Click **Advanced Sharing**
5. Enable sharing

Share name:

```
downloads
```

Permissions:

```
Everyone → Full Control
```

Only the **downloads** folder should be shared, not the entire `C:\data`.

---

## Create a Test File

Inside the folder:

```
C:\data\downloads
```

Create the file:

```
publicVonWindows.txt
```

Example content:

```
Hello from Windows
```

---

# 3. Linux Configuration

## Create Mount Directory

On Linux run:

```
sudo mkdir /mnt/smb
```

This directory will be used as the mount point.

---

# 4. Mount the Windows Share

Run the following command:

```
sudo mount //WINDOWS_IP/downloads /mnt/smb -o username=vmadmin,dir_mode=0777,file_mode=0777
```

Example:

```
sudo mount //192.168.210.132/downloads /mnt/smb -o username=vmadmin,dir_mode=0777,file_mode=0777
```

Explanation:

- `//192.168.210.132/downloads` → Windows shared folder  
- `/mnt/smb` → Linux mount point  
- `username=vmadmin` → Windows username used for authentication  
- `dir_mode=0777` → permissions for directories  
- `file_mode=0777` → permissions for files  

---

# 5. Verify Access

List files:

```
ls /mnt/smb
```

Expected output:

```
publicVonWindows.txt
```

---

# 6. Read the File

```
cat /mnt/smb/publicVonWindows.txt
```

You should see the content from the Windows file.

---

# 7. Modify the File from Linux

```
echo "Input from Linux" >> /mnt/smb/publicVonWindows.txt
```

The file on the Windows server will now contain:

```
Hello from Windows
Input from Linux
```

---

# 8. Unmount the Share

After finishing the exercise:

```
sudo umount /mnt/smb
```

---

# Protocol Used

The connection uses the SMB protocol.

Protocol stack:

```
Ethernet
IP
TCP
SMB (Port 445)
```

---

# Result

The Linux system successfully connects to a Windows shared folder using SMB and can:

- read files
- write files
- modify files
