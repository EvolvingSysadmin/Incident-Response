---
title: Memory Analysis with Volatility
permalink: /docs/memory-analysis-with-volatility/
---

ISC2 Memory Analysis with Volatility Class

## Volatility Background
* Volatility is a memory analysis tool for live memory capture and analysis
* Written in Python

## Configuring and launching volatility

Launch Volatility
```shell
python /usr/share/volatility/vol.py
```

Show help/commands
```shell
volatility -h
```

Run image memory tool at memory image file: img0.mem
```shell
volatility imageinfo -f images/img0.mem
```

Get suggested profiles and useful data structures
Data Structures: KDBG, KPCR, DTB

Create configuration file in current directory with suggested profile information
the file is volatilityrc
Run volatility pslist
this tool lists all running processes with memory offset, PID, parent PID, number of threads/handles, start/stop time

## Examining Processes, DLLs, and Commands

volatility pstree: like pslist but shows intetended trees to show parent child process relationships

volatility psscan

Searches entirety of RAM for process headers
Can find processes that have terminated or are hidden

volatility procdump -D dumps
Procdump copies process executible contents from memory to disk

-p PID limits to a single process

-u or --unsafe can help find more executibles by bypasses checks

ls -l dumps to examine dumps directory contents for exes

volatility dlllist

shows dlls for each running process from target image, along with the base address, path, size for each dll

volatility dlldump -D dumps -p $PID

extracts DLLs for a specific process

DLLs get reloaded from disk by the process when the process is used

volatility cmdscan

lists console commands executed on host processes

volatility consoles

scabs fir data structures in memory, shows the tools used to create the memory snapshot itself (eg dd)

## Analyze Network Activity

volatility connections

shows active tcp connections

volatility sockets

shows open sockets listening for connections, so could be TCP, UDP, or RAW sockets

volatility netscan

shows similar info and ipv6 connections

## Analyze Windows Registry

volatility hivelist

lists the virtual addresses of each registry hive in memory and shows corresponding locations on disk

Windows stores registry data in a collection of binary hive files

volatility hivedump --output-file hivedump.txt --hive-offset address

volatility printkey -K 'Microsoft\Windows\CurrentVersion\Run'

shows a specific registry keys values and data

'Microsoft\Windows\CurrentVersion\Run'

The Windows registry includes the following four keys:

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce

volatility dumpregistry -D dumps

Dumps entire contents of registry. Writes windows binary registry files with all keys, subkeys, values and associated data. The resulting files are readable by tools that can parse windows nt registry file formats.

These are in the same file format as NTUSER.DAT

they are binary files


## Search for malware artifacts

- use volatility imageinfo to get info about memory image file to build a new volatility vonfiguration file

- use leafpad/text editor to make the configuration file: volatilityrc
with configs necesseary to run volatility against desired memory image

- volatility malfind

shows suspicious pages found in the file: shows process name, pid, and virtual address

- enumerate kernel drivers

volatility modules

lists kernel drivers loaded in the system by going through the kernels own list of drivers

kenel drivers run at lowest level of os with root privs

volatility driverirp can show functions a driver provides

- volatility modscan

can find hidden drivers that have unlinked from the kernels list

scans RAM for driver table entry structures

any discrepancies between modules and modscan should be investigated

- enumerate hidden processes

volatility psxview

uses 6 different techniques to search raw memory for processes

- look for hidden, unlinked dlls (unlinked from the process environment block PEB)

- volatility ldrmodules

scans virtual address decriptor table (as in malfind) and references that with the Process Environment Blocks

it can be narrowed down to particularly processes PIDs:

volatility ldrmodules -p PID -v (verbose)

- volatility callbacks

showss when a driver has registered to be notified of an event

## Volatility Forensic Analysis Tools

- volatility evtlogs -D dumps

event log tool to extract binary event logs from Win XP and 2003, even if not saved to disk

- volatility svcscan

lists all running services; can include kernel drivers and user mode processes

shows PID, service name, display name, type, status

- volatility handles -p PID --silent 

Displays which handles a process has open, which includes files, registry keys, windows stations and events, among other handles

- volatility timeliner --output-file timeline.txt

import into spreadsheet program or can do basic sorting within terminal with gedit:

sort timeline.txt | gedit -

Generates a timeline based on process launches/exits, socket create times, event log entries and other events found through memory

## Quiz

* & allows one to put a process in the background
* system does not have a session id
* >& redirects output and appends a file
* session manager does not have a session id (smss.exe)
* use profiles for memory dumps