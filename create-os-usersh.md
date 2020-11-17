---

copyright:
  years: 2020
lastupdated: "2020-11-05"

keywords: script, create_os_user, non-root- user, enable SSH login, install, websphere liberty, vsi

subcollection: was-for-vsi

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:term: .term}



# Script for a non-root user and SSH login
{: #create_os_usersh}

You can use the following `create_os_user.sh` script to create a non-root user and enable SSH login.
{: shortdesc}

```bash
# Name: create_os_user.sh
# Purpose: Create a non-root user and setup sudo and SSH access.
#

if [ -z "$2" ]; then
    echo -e "Usage: $0 <user-name> <user-password>"
    exit 1
fi

currentTime=$(date "+%Y-%m-%d-%H-%M-%S")
echo currentTime=$currentTime

echo Adding user $1
useradd $1
echo $2 | passwd $1 --stdin
usermod -aG wheel $1

fileName="/etc/sudoers"
echo "Backing up $fileName to $fileName.$currentTime"
cp $fileName $fileName.$currentTime
echo Enabling sudo root access for $1
echo "$1     ALL=(ALL)     NOPASSWD: ALL" >> $fileName

fileName="/etc/ssh/sshd_config"
echo "Backing up $fileName to $fileName.$currentTime"
cp $fileName $fileName.$currentTime
echo Enabling password login for $1
echo "Match User $1" >> $fileName
echo "        PasswordAuthentication yes" >> $fileName

echo Restarting sshd service
service sshd restart
```
{: codeblock}
