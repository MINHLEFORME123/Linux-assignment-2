# Linux User Management and File Permissions Assignment

## Introduction
This document outlines the steps taken to create users, configure a system user, grant sudo privileges, and set up a shared directory with advanced group permissions on a Linux Virtual Machine.

---

## 1. Create User: Tupu
I used the `adduser` script to create the user `tupu`. This script is interactive and automatically creates the home directory and asks for a password.

**Command:**
```bash
sudo adduser tupu
```

<img width="822" height="612" alt="image" src="https://github.com/user-attachments/assets/ad1f32ba-6688-4e0c-95f7-7fab2bead732" />
## 2. Create Lupu User
I used the useradd command with specific flags to create the user profile, home directory, and shell. Then I set the password manually.

Commands:

```Bash
sudo useradd -m -d /home/lupu -s /bin/bash -G lupu lupu
sudo passwd lupu
```
## 3. Create Hupu System User
I created a system user named hupu with its login shell set to /bin/false to prevent any interactive login.

Command:

```Bash
sudo useradd --system --shell /bin/false hupu
```
## 4. Grant Sudo Privileges
I added both tupu and lupu to the sudo group to allow them to execute administrative commands.

Commands:

```Bash
sudo usermod -aG sudo tupu
sudo usermod -aG sudo lupu
```
## 5. Directory Permissions and Collaboration (/opt/projekti)
Implementation Explanation
To ensure only Tupu and Lupu can access the directory and that all new files maintain the correct group ownership:

Shared Group: I created a group named projekti and added both users to it.

Access Control: I set the permissions to 770 (Full access for Owner/Group, none for others).

SetGID Bit: I applied the setgid bit (chmod g+s). This ensures any file created inside this directory automatically belongs to the projekti group, enabling seamless collaboration.

Commands:

```Bash
sudo groupadd projekti
sudo usermod -aG projekti tupu
sudo usermod -aG projekti lupu
sudo mkdir -p /opt/projekti
sudo chown :projekti /opt/projekti
sudo chmod 770 /opt/projekti
sudo chmod g+s /opt/projekti
```
## 6. Testing and Verification
I verified the configuration by switching to user tupu and creating a file to test the group inheritance (SetGID).

Commands:

```Bash
su - tupu
cd /opt/projekti
touch verify_test.txt
ls -l
```
##Result: The output of ls -l confirms that verify_test.txt belongs to the group projekti, proving the permission structure is maintained.
