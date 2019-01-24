<p align="center">
    <img src="https://github.com/trimstray/working-template/blob/master/doc/img/main_preview.png"
        alt="Master">
</p>

<br>

<p align="center">
  <a href="https://github.com/trimstray/linux-hardening-checklist/tree/master">
    <img src="https://img.shields.io/badge/Branch-master-green.svg?longCache=true"
        alt="Branch">
  </a>
  <a href="https://github.com/trimstray/working-template/pulls">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?longCache=true"
        alt="Pull Requests">
  </a>
  <a href="http://www.gnu.org/licenses/">
    <img src="https://img.shields.io/badge/License-GNU-blue.svg?longCache=true"
        alt="License">
  </a>
</p>

<div align="center">
  <sub>Created by
  <a href="https://twitter.com/trimstray">trimstray</a> and
  <a href="https://github.com/trimstray/linux-hardening-checklist/graphs/contributors">
    contributors
  </a>
</div>

<br>

****

# Table of Contents

- **[Introduction](#introduction)**
  * [General disclaimer](#general-disclaimer)
  * [Levels of priority](#levels-of-priority)
  * [OpenSCAP](#openscap)
- **[Partitioning](#partitioning)**
  * [Separate partitions](#separate-partitions)
  * [Restrict mount options](#restrict-mount-options)
  * [Polyinstantiated directories](#polyinstantiated-directories)
  * [Shared memory](#shared-memory)
  * [Encrypt partitions](#encrypt-partitions)
  * [:ballot_box_with_check: Summary checklist](#ballot-box-with-check-summary-checklist)
  * [Summary checklist](#ballot_box_with_check-summary-checklist)
- **[Bootloader](#bootloader)**
  * [Protect bootloader config files](#protect-bootloader-config-files)
- **[Linux Kernel](#linux-kernel)**
- **[Logging](#logging)**
- **[Users and Groups](#users-and-groups)**
- **[Permissions](#permissions)**
- **[SELinux & Auditd](#selinux--auditd)**
- **[System Updates](#system-updates)**
- **[Network](#network)**
- **[Services](#services)**
- **[Tools](#tools)**

# Introduction

  > In computing, **hardening** is usually the process of securing a system by reducing its surface of vulnerability, which is larger when a system performs more functions; in principle a single-function system is more secure than a multipurpose one. The main goal of systems hardening is to reduce security risk by eliminating potential attack vectors and condensing the system’s attack surface.

## General disclaimer

I'm not advocating throwing your existing hardening and deployment best practices out the door but I recommend is to always turn a feature from this checklist on in pre-production environments instead of jumping directly into production.

## Levels of priority

All items in this checklist contains three levels of priority:

* <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> means that the item has a **low** priority.
* <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> means that the item has a **medium** priority. You shouldn't avoid tackling that item.
* <img src="https://github.com/trimstray/working-template/blob/master/doc/img/high.png" alt="high"> means that the item has a **high** priority. You can't avoid following that rule and implement the corrections recommended.

## OpenSCAP

<img src="https://github.com/trimstray/working-template/blob/master/doc/img/openscap_logo.png" alt="OpenSCAP" align="left">

<p align="left"><b>SCAP</b> (<i>Security Content Automation Protocol</i>) provides a mechanism to check configurations, vulnerability management and evaluate policy compliance for a variety of systems. One of the most popular implementations of SCAP is <b>OpenSCAP</b> and it is very helpful for vulnerability assessment and also as hardening helper.

Some of the external audit tools use this standard. For example Nessus has functionality for authenticated SCAP scans.</p>

  > I tried to make this list fully compatible with OpenSCAP standard and rules.

# Partitioning

## Separate partitions

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Ensure `/boot` located on separate partition.

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Ensure `/home` located on separate partition.

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Ensure `/usr` located on separate partition.

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/high.png" alt="high"> Ensure `/var` located on separate partition.

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/high.png" alt="high"> Ensure `/var/log` and `/var/log/audit` located on separate partitions.

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/high.png" alt="high"> Ensure `/tmp` and `/var/tmp` located on separate partitions.

## Restrict mount options

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Restrict `/usr` partition mount options.

    **Example:**

    ```bash
    UUID=<...>  /usr  ext4  defaults,nodev,ro  0 2
    ```

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Restrict `/var` partition mount options.

    **Example:**

    ```bash
    UUID=<...>  /var  ext4  defaults,nosuid  0 2
    ```

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Restrict `/var/log` and `/var/log/audit` partitions mount options.

    **Example:**

    ```bash
    UUID=<...>  /var/log        ext4  defaults,nosuid,noexec,nodev  0 2
    UUID=<...>  /var/log/audit  ext4  defaults,nosuid,noexec,nodev  0 2
    ```

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Restrict `/proc` partition mount options.

    **Example:**

    ```bash
    proc  /proc  proc  defaults,hidepid=2  0 0
    ```

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> Restrict `/boot` partition mount options.

    **Example:**

    ```bash
    LABEL=/boot  /boot  ext2  defaults,nodev,nosuid,noexec,ro  1 2
    ```

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> Restrict `/home` partition mount options.

    **Example:**

    ```bash
    UUID=<...>  /home  ext4  defaults,nodev,nosuid  0 2
    ```

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> Restrict `/var` and `/var/tmp` partitions mount options.

    **Example:**

    ```bash
    mv /var/tmp /var/tmp.old
    ln -s /tmp /var/tmp
    cp -prf /var/tmp.old/* /tmp && rm -fr /var/tmp.old

    UUID=<...>  /tmp  ext4  defaults,nodev,nosuid,noexec  0 2
    ```

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> Restrict `/dev/shm` partition mount options.

    **Example:**

    ```bash
    tmpfs  /dev/shm  tmpfs  rw,nodev,nosuid,noexec,size=1024M,mode=1777 0 0
    ```

## Polyinstantiated directories

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> Setting up polyinstantiated `/var` and `/var/tmp` directories.

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

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Set group for `/dev/shm`.

    **Example:**

    ```bash
    tmpfs  /dev/shm  tmpfs  rw,nodev,nosuid,noexec,size=1024M,mode=1770,uid=root,gid=shm 0 0
    ```

## Encrypt partitions

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Encrypt `swap` partition.

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
| separate `/boot` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| separate `/home` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| separate `/usr` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| separate `/var` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/high.png" alt="high"> | :black_square_button: |
| separate `/var/log` and `/var/log/audit` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/high.png" alt="high"> | :black_square_button: |
| separate `/tmp` and `/var/tmp` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/high.png" alt="high"> | :black_square_button: |
| restrict `/usr` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| restrict `/var` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| restrict `/var/log` and `/var/log/audit` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| restrict `/proc` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| restrict `/boot` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> | :black_square_button: |
| restrict `/home` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> | :black_square_button: |
| restrict `/tmp/` and `/var/tmp` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> | :black_square_button: |
| restrict `/dev/shm` mount options | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> | :black_square_button: |
| polyinstantiated `/tmp` and `/var/tmp` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/medium.png" alt="medium"> | :black_square_button: |
| set group for `/dev/shm` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |
| encrypt `swap` | <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> | :black_square_button: |

# Bootloader

## Protect bootloader config files

- <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low"> Ensure bootloader config files are set properly permissions.

    **Example:**

    ```bash
    # Set the owner and group of /etc/grub.conf to the root user:
    chown root:root /etc/grub.conf
    chown -R root:root /etc/grub.d

    # Set permissions on the /etc/grub.conf or /etc/grub.d file to read and write for root only:
    chmod og-rwx /etc/grub.conf
    chmod -R og-rwx /etc/grub.d
    ```

# Linux Kernel

# Logging

# Users and Groups

# Permissions

# SELinux & Auditd

# System Updates

# Network

# Services

# Tools
