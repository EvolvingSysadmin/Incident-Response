---
title: File System Forensics
permalink: /docs/file-system-forensics/
---

## Background

ISC2 File System Forensics: <https://www.isc2.org/Development/Lab-Courses/Introductory-File-System-Forensics>

Certificate of Completion:

https://github.com/EvolvingSysadmin/Incident-Response/blob/master/assets/ISC2-Introductory-File-Systems-Forensics-Course-Completion-Certificate.pdf

Tools
* Autopsy disk imaging/analysis tools
* AccessData's free FTK Imager for generating an image
* https://toolcatalog.nist.gov/ 

Hardware tools
* Hardware write blocker
* Liquid nitrogen
* Ice
* Bootable USB stick

Sleuthkit in Unix
* Analyze image made using FTK imager: .raw .img
* Show disk image partition tables: mmls IMAGE*FILE
* Show metadata for partition: fstat *o START*SECTOR*NUMBER IMAGE*FILE
  * Deleted files and other data could be in free sector space
* List contents: fls *o START*SECTOR*NUMBER IMAGE*FILE
  * D: directory
  * R: regular file
  * Number: inode number
* Show file contents: icat *o START*SECTOR*NUMBER IMAGE*FILE FILE*INODE
  *  before inode = deleted file

Autopsy

* EnCase and FTK are commercial equivalents
* Ensure keyword search module has everything enabled in it
* Uses byte ranges
* Views → Deleted to view deleted files
* Can view files carved from raw disk
