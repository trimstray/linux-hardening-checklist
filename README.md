<p align="center">
  <a href="https://github.com/trimstray/linux-hardening-checklist">
    <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/linux-hardening-checklist_preview.png" alt="Master">
  </a>
</p>

<br>

<p align="center">
  <a href="https://github.com/trimstray/linux-hardening-checklist/pulls">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?longCache=true" alt="Pull Requests">
  </a>
  <a href="LICENSE.md">
    <img src="https://img.shields.io/badge/License-MIT-lightgrey.svg?longCache=true" alt="MIT License">
  </a>
</p>

<p align="center">
  <a href="https://twitter.com/trimstray" target="_blank">
    <img src="https://img.shields.io/twitter/follow/trimstray.svg?logo=twitter">
  </a>
</p>

<div align="center">
  <sub>Created by
  <a href="https://twitter.com/trimstray">trimstray</a> and
  <a href="https://github.com/trimstray/linux-hardening-checklist/graphs/contributors">contributors</a>
</div>

<br>

****

# Table of Contents

- **[Introduction](#introduction)**
  * [Status](#status)
  * [Todo](#todo)
  * [Prologue](#prologue)
  * [Levels of priority](#levels-of-priority)
  * [OpenSCAP](#openscap)
- **[Partitioning](#partitioning)**
  * [Separate partitions](#separate-partitions)
  * [Restrict mount options](#restrict-mount-options)
  * [Polyinstantiated directories](#polyinstantiated-directories)
  * [Shared memory](#shared-memory)
  * [Encrypt partitions](#encrypt-partitions)
  * [Summary checklist](#ballot_box_with_check-summary-checklist)
- **[Physical Access](#physical-access)**
  * [Password for Single User Mode](#password-for-single-user-mode)
  * [Summary checklist](#ballot_box_with_check-summary-checklist-1)
- **[Bootloader](#bootloader)**
  * [Protect bootloader config files](#protect-bootloader-config-files)
  * [Summary checklist](#ballot_box_with_check-summary-checklist-2)
- **[Linux Kernel](#linux-kernel)**
  * [Kernel logs](#kernel-logs)
  * [Kernel pointers](#kernel-pointers)
  * [ExecShield](#execshield)
  * [Memory protection](#memory-protection)
  * [Summary checklist](#ballot_box_with_check-summary-checklist-3)
- **[Logging](#logging)**
  * [Syslog](#syslog)
- **[Users and Groups](#users-and-groups)**
  * [Passwords](#passwords)
  * [Logon Access](#logon-access)
  * [Summary checklist](#ballot_box_with_check-summary-checklist-4)
- **[Filesystem](#filesystem)**
  * [Hardlinks & Symlinks](#hardlinks--symlinks)
  * [Dynamic Mounting and Unmounting](#dynamic-mounting-and-unmounting)
  * [Summary checklist](#ballot_box_with_check-summary-checklist-5)
- **[Permissions](#permissions)**
- **[SELinux & Auditd](#selinux--auditd)**
  * [SELinux Enforcing](#selinux-enforcing)
  * [Summary checklist](#ballot_box_with_check-summary-checklist-6)
- **[System Updates](#system-updates)**
- **[Network](#network)**
  * [TCP/SYN](#tcp-syn)
  * [Routing](#routing)
  * [ICMP Protocol](#icmp-protocol)
  * [Broadcast](#broadcast)
  * [Summary checklist](#ballot_box_with_check-summary-checklist-7)
- **[Services](#services)**
- **[Tools](#tools)**

# Introduction

  > In computing, **hardening** is usually the process of securing a system by reducing its surface of vulnerability, which is larger when a system performs more functions; in principle a single-function system is more secure than a multipurpose one. The main goal of systems hardening is to reduce security risk by eliminating potential attack vectors and condensing the system’s attack surface.

This list contains the most important hardening rules for GNU/Linux systems.

## Status

Still work in progress... :construction_worker:

I also created another repository (in a more detailed way): [the-practical-linux-hardening-guide](https://github.com/trimstray/the-practical-linux-hardening-guide).

## Todo

- [ ] Add rationale (e.g. url's, external resources)
- [ ] Review levels of priority

## Prologue

I'm not advocating throwing your existing hardening and deployment best practices out the door but I recommend is to always turn a feature from this checklist on in pre-production environments instead of jumping directly into production.

## Levels of priority

All items in this checklist contains three levels of priority:

* <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> means that the item has a **low** priority.
* <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> means that the item has a **medium** priority. You shouldn't avoid tackling that item.
* <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/high.png" alt="high"> means that the item has a **high** priority. You can't avoid following that rule and implement the corrections recommended.

## OpenSCAP

<img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/openscap_logo.png" alt="OpenSCAP" align="left">

<p align="left"><b>SCAP</b> (<i>Security Content Automation Protocol</i>) provides a mechanism to check configurations, vulnerability management and evaluate policy compliance for a variety of systems. One of the most popular implementations of SCAP is <b>OpenSCAP</b> and it is very helpful for vulnerability assessment and also as hardening helper.

Some of the external audit tools use this standard. For example Nessus has functionality for authenticated SCAP scans.</p>

  > I tried to make this list compatible with OpenSCAP standard and rules. However, there may be differences.

# Partitioning

## Separate partitions

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Ensure `/boot` located on separate partition.

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Ensure `/home` located on separate partition.

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Ensure `/usr` located on separate partition.

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Ensure `/var` located on separate partition.

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/high.png" alt="high"> Ensure `/var/log` and `/var/log/audit` located on separate partitions.

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/high.png" alt="high"> Ensure `/tmp` and `/var/tmp` located on separate partitions.

## Restrict mount options

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Restrict `/usr` partition mount options.

    **Example:**

    ```bash
    UUID=<...>  /usr  ext4  defaults,nodev,ro  0 2
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Restrict `/var` partition mount options.

    **Example:**

    ```bash
    UUID=<...>  /var  ext4  defaults,nosuid  0 2
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Restrict `/var/log` and `/var/log/audit` partitions mount options.

    **Example:**

    ```bash
    UUID=<...>  /var/log        ext4  defaults,nosuid,noexec,nodev  0 2
    UUID=<...>  /var/log/audit  ext4  defaults,nosuid,noexec,nodev  0 2
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Restrict `/proc` partition mount options.

    **Example:**

    ```bash
    proc  /proc  proc  defaults,hidepid=2  0 0
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Restrict `/boot` partition mount options.

    **Example:**

    ```bash
    LABEL=/boot  /boot  ext2  defaults,nodev,nosuid,noexec,ro  1 2
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Restrict `/home` partition mount options.

    **Example:**

    ```bash
    UUID=<...>  /home  ext4  defaults,nodev,nosuid  0 2
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Restrict `/var` and `/var/tmp` partitions mount options.

    **Example:**

    ```bash
    mv /var/tmp /var/tmp.old
    ln -s /tmp /var/tmp
    cp -prf /var/tmp.old/* /tmp && rm -fr /var/tmp.old

    UUID=<...>  /tmp  ext4  defaults,nodev,nosuid,noexec  0 2
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Restrict `/dev/shm` partition mount options.

    **Example:**

    ```bash
    tmpfs  /dev/shm  tmpfs  rw,nodev,nosuid,noexec,size=1024M,mode=1777 0 0
    ```

## Polyinstantiated directories

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Setting up polyinstantiated `/var` and `/var/tmp` directories.

    **Example:**

    ```bash
    # Create new directories:
    mkdir --mode 000 /tmp-inst
    mkdir --mode 000 /var/tmp/tmp-inst

    # Edit /etc/security/namespace.conf:
    /tmp      /tmp-inst/          level  root,adm
    /var/tmp  /var/tmp/tmp-inst/  level  root,adm

    # Set correct SELinux context:
    setsebool polyinstantiation_enabled=1
    chcon --reference=/tmp /tmp-inst
    chcon --reference=/var/tmp/ /var/tmp/tmp-inst
    ```

## Shared memory

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Set group for `/dev/shm`.

    **Example:**

    ```bash
    tmpfs  /dev/shm  tmpfs  rw,nodev,nosuid,noexec,size=1024M,mode=1770,uid=root,gid=shm 0 0
    ```

## Encrypt partitions

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Encrypt `swap` partition.

    **Example:**

    ```bash
    # Edit /etc/crypttab:
    sdb1_crypt /dev/sdb1 /dev/urandom cipher=aes-xts-plain64,size=256,swap,discard

    # Edit /etc/fstab:
    /dev/mapper/sdb1_crypt none swap sw 0 0
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Separate `/boot` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Separate `/home` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Separate `/usr` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Separate `/var` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Separate `/var/log` and `/var/log/audit` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/high.png" alt="high"> | :black_square_button: |
| Separate `/tmp` and `/var/tmp` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/high.png" alt="high"> | :black_square_button: |
| | | |
| Restrict `/usr` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Restrict `/var` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Restrict `/var/log` and `/var/log/audit` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Restrict `/proc` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Restrict `/boot` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Restrict `/home` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Restrict `/tmp/` and `/var/tmp` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Restrict `/dev/shm` mount options | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| | | |
| Polyinstantiated `/tmp` and `/var/tmp` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| | | |
| Set group for `/dev/shm` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| | | |
| Encrypt `swap` | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |

# Physical Access

## Password for Single User Mode

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Protect Single User Mode with root password.

    **Example:**

    ```bash
    # Edit /etc/sysconfig/init.
    SINGLE=/sbin/sulogin
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Protect Single User Mode. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |

# Bootloader

## Protect bootloader config files

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Ensure bootloader config files are set properly permissions.

    **Example:**

    ```bash
    # Set the owner and group of /etc/grub.conf to the root user:
    chown root:root /etc/grub.conf
    chown -R root:root /etc/grub.d

    # Set permissions on the /etc/grub.conf or /etc/grub.d file to read and write for root only:
    chmod og-rwx /etc/grub.conf
    chmod -R og-rwx /etc/grub.d
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Protect bootloader config files | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |

# Linux Kernel

## Kernel logs

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Restricting access to kernel logs.

    **Example:**

    ```bash
    echo "kernel.dmesg_restrict = 1" > /etc/sysctl.d/50-dmesg-restrict.conf
    ```

## Kernel pointers

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Restricting access to kernel pointers.

    **Example:**

    ```bash
    echo "kernel.kptr_restrict = 1" > /etc/sysctl.d/50-kptr-restrict.conf
    ```

## ExecShield

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> ExecShield protection.

    **Example:**

    ```bash
    echo "kernel.exec-shield = 2" > /etc/sysctl.d/50-exec-shield.conf
    ```

## Memory protections

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Randomise memory space.

    ```bash
    echo "kernel.randomize_va_space=2" > /etc/sysctl.d/50-rand-va-space.conf
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Restricting access to kernel logs | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Restricting access to kernel pointers | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| ExecShield protection | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Randomise memory space. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |

# Logging

## Syslog

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Ensure syslog service is enabled and running.

    **Example:**

    ```bash
    systemctl enable rsyslog
    systemctl start rsyslog
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Send syslog data to external server.

    **Example:**

    ```bash
    # ELK
    # Logstash
    # Splunk
    # ...
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Ensure syslog service is enabled and running. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Ensure syslog service is enabled and running. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |

# Users and Groups

## Passwords

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Update password policy (PAM).

    **Example:**

    ```bash
    authconfig --passalgo=sha512 \
    --passminlen=14 \
    --passminclass=4 \
    --passmaxrepeat=2 \
    --passmaxclassrepeat=2 \
    --enablereqlower \
    --enablerequpper \
    --enablereqdigit \
    --enablereqother \
    --update
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Limit password reuse (PAM).

    **Example:**

    ```bash
    # Edit /etc/pam.d/system-auth

    # For the pam_unix.so case:
    password sufficient pam_unix.so ... remember=5

    # For the pam_pwhistory.so case:
    password requisite pam_pwhistory.so ... remember=5
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Secure `/etc/login.defs` password policy.

    **Example:**

    ```bash
    # Edit /etc/login.defs
    PASS_MIN_LEN 14
    PASS_MIN_DAYS 1
    PASS_MAX_DAYS 60
    PASS_WARN_AGE 14
    ```

## Logon Access

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Set auto logout inactive users.

    **Example:**

    ```bash
    echo "readonly TMOUT=900" >> /etc/profile.d/idle-users.sh
    echo "readonly HISTFILE" >> /etc/profile.d/idle-users.sh
    chmod +x /etc/profile.d/idle-users.sh
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Set last logon/access notification.

    **Example:**

    ```bash
    # Edit /etc/pam.d/system-auth
    session required pam_lastlog.so showfailed
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Lock out accounts after a number of incorrect login (PAM).

    **Example:**

    ```bash
    # Edit /etc/pam.d/system-auth and /etc/pam.d/password-auth

    # Add the following line immediately before the pam_unix.so statement in the AUTH section:
    auth required pam_faillock.so preauth silent deny=3 unlock_time=never fail_interval=900

    # Add the following line immediately after the pam_unix.so statement in the AUTH section:
    auth [default=die] pam_faillock.so authfail deny=3 unlock_time=never fail_interval=900

    # Add the following line immediately before the pam_unix.so statement in the ACCOUNT section:
    account required pam_faillock.so
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Update password policy | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Limit password reuse | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Secure `/etc/login.defs` password policy | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| | | |
| Set auto logout inactive users. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Set last logon/access notification | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Lock out accounts after a number of incorrect login | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |

# Filesystem

## Hardlinks & Symlinks

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> Enable hard/soft link protection.

    **Example:**

    ```bash
    echo "fs.protected_hardlinks = 1" > /etc/sysctl.d/50-fs-hardening.conf
    echo "fs.protected_symlinks = 1" >> /etc/sysctl.d/50-fs-hardening.conf
    ```

## Dynamic Mounting and Unmounting

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Disable uncommon filesystems.

    **Example:**

    ```bash
    echo "install cramfs /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install freevxfs /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install jffs2 /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install hfs /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install hfsplus /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install squashfs /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install udf /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install fat /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install vfat /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install nfs /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install nfsv3 /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    echo "install gfs2 /bin/false" > /etc/modprobe.d/uncommon-fs.conf
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Enable hard/soft link protection. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/low.png" alt="low"> | :black_square_button: |
| Disable uncommon filesystems. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |

# Permissions

# SELinux & Auditd

## SELinux Enforcing

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/high.png" alt="high"> Set SELinux Enforcing mode.

    **Example:**

    ```bash
    # Edit /etc/selinux/config.
    SELINUXTYPE=enforcing
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Set SELinux Enforcing mode. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/high.png" alt="high"> | :black_square_button: |

# System Updates

# Network

## TCP/SYN

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Enable TCP SYN Cookie protection.

    **Example:**

    ```bash
    echo "net.ipv4.tcp_syncookies = 1" > /etc/sysctl.d/50-net-stack.conf
    ```

## Routing

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Disable IP source routing.

    **Example:**

    ```bash
    echo "net.ipv4.conf.all.accept_source_route = 0" > /etc/sysctl.d/50-net-stack.conf
    ```

## ICMP Protocol

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Disable ICMP redirect acceptance.

    **Example:**

    ```bash
    echo "net.ipv4.conf.all.accept_redirects = 0" > /etc/sysctl.d/50-net-stack.conf
    ```

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Enable ignoring to ICMP requests.

    **Example:**

    ```bash
    echo "net.ipv4.icmp_echo_ignore_all = 1" > /etc/sysctl.d/50-net-stack.conf
    ```

## Broadcast

- <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> Enable ignoring broadcasts request.

    **Example:**

    ```bash
    echo "net.ipv4.icmp_echo_ignore_broadcasts = 1" > /etc/sysctl.d/50-net-stack.conf
    ```

## :ballot_box_with_check: Summary checklist

| <b>Rule</b> | <b>Priority</b> | <b>Checkbox</b> |
| :---        | :---:       | :---:        |
| Enable TCP SYN Cookie protection. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| | | |
| Disable IP source routing. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| | | |
| Disable ICMP redirect acceptance. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| Enable ignoring to ICMP requests. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |
| | | |
| Enable ignoring broadcasts request. | <img src="https://github.com/trimstray/linux-hardening-checklist/blob/master/static/img/medium.png" alt="medium"> | :black_square_button: |

# Services

# Tools
