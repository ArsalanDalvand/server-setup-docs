---
title: "pveperf(1)"
source: "https://pve.proxmox.com/pve-docs/pveperf.1.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pveperf.1"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pveperf(1)"
---
[â](#)

## NAME

pveperf - the Proxmox benchmark

## SYNOPSIS

pveperf [PATH]

## DESCRIPTION

Tries to gather some CPU/hard disk performance data on the hard disk
mounted at PATH (/ is used as default):

CPU BOGOMIPS

bogomips sum of all CPUs

REGEX/SECOND

regular expressions per second (perl performance test), should
be above 300000

HD SIZE

hard disk size

BUFFERED READS

simple HD read test. Modern HDs should reach at least 40
MB/sec

AVERAGE SEEK TIME

tests average seek time. Fast SCSI HDs reach values < 8
milliseconds. Common IDE/SATA disks get values from 15 to 20 ms.

FSYNCS/SECOND

value should be greater than 200 (you should enable write
back cache mode on you RAID controller - needs a battery backed cache (BBWC)).

DNS EXT

average time to resolve an external DNS name

DNS INT

average time to resolve a local DNS name