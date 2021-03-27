---
title: Volatile Memory Analysis with Rekall
permalink: /docs/memory-analysis-with-rekall/
---

## Class Background

ISC2 Introduction to Memory Analysis with Rekall: <https://www.isc2.org/Development/Lab-Courses/Introduction-to-Memory-Analysis-with-Rekall>

Rekall is a memory forensic and incident response framework: <http://www.rekall-forensic.com/>
- <https://github.com/google/rekall>

## Rekall Command Line Basics

Get Image Info:
```rekall -f images/zeus.vmem imageinfo```

Get User Info from Image:
```rekall -f images/zeus.vmem users```

desktop processes in session of image
rekall -f images/zeus.vmem users

run mimikatz to see what can be extracted from system of image
rekall -f images/zeus.vmem mimikatz

## Rekall Live Mode

Rekall in live mode v RAM snapshot

sudo rekall --live Memory

    arp

    plugins.TAB = autocompletion options

    pslist = list running processes

    pstree = running processes in a tree

    lsof = shows all files open by all processes

    bash = searches for bash command history

## Rekall Malware Analysis of Zeus Trojan

https://en.wikipedia.org/wiki/Zeus_(malware)

Taken down with Operation Tovar

rekall -f images/zeus.vmem

pslsit

cmdscan: searches for commands entered in windows console

connscan: active/recent TCP connections

Online IP research
who.is
ipvoid

pslist(pids=856) = search for particular process (PID = 856 in this case)

svchost.exe (manages windows services)
if starts at boot = persistence

printkey key="REGISTRY\KEY" = checks registry info, search for .exe in registry

procinfo(pids=856) = detailed info about a process

malfind = looks for properties of malware throughout system

malfind pids=856, dump_dir="."

## Rekall Malware Analysis of Tigger 

Tigger is an advanced malware package targeting the financial services sector. Uses privilege escalation to get admin access and install a rootkit.

### Analysis Command History
 * pslist
 * psxview: running processes in multiple ways
 * cmdscan: current/past TCP connections
 * driverscan: searches drivers
 * callbacks: searches for callback functions registered with kernel
 * printkey key="ControlSet001\Services\SharedAccess\Parameters\FirewallPolicy\StandardProfile": check firewall in registry

## Coreflood Memory Analysis

Coreflood is a trojan and botnet that includes a keylogged to exfiltrate information via the keylogged.

2044 = child process of internet explorer

### Analysis Command History
 * rekall -f images/coreflood.vmem --pager leafpad
 * imageinfo
 * pslist
 * procinfo 2044
 * pslist 2044
 * handles 2044 = shows open handles
 * hooks_iat 2044 = shows hooks
 * malfind 2044 (malware removes headers to prevent assembly level analysis)

## Other

* Prefetch files = 128 files prefetch in winxp - win7
* csrcs.exe is evidence of malware
* Computer\H KEY _LOCAL_MACH IN E\SYSTEM\CurrentControlSet\services\eventlog\Start = 4 = disable
* NDIS driver can hide network activity
* services.exe runs from C:\Windows\System32 in winxp and win server 2003