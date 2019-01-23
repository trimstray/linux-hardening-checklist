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
- **[Bootloader](#bootloader)**
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

## Partitioning

### /boot partition

**Priority:** <img src="https://github.com/trimstray/working-template/blob/master/doc/img/low.png" alt="low">

**Rationale:**

The idea behind the `/boot` partition was to make the partition always accessible to any machine that the drive was plugged into. As modern machines have lifted that restriction, there is no longer a fixed need for `/boot` to be separate, unless you require additional processing of the other partitions, such as encryption or file systems that are not natively recognized by the bootloader.

# Bootloader

# Linux Kernel

# Logging

# Users and Groups

# Permissions

# SELinux & Auditd

# System Updates

# Network

# Services

# Tools
