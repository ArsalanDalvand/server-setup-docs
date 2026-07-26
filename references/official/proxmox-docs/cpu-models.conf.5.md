---
title: "cpu-models.conf(5)"
source: "https://pve.proxmox.com/pve-docs/cpu-models.conf.5.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cpu-models.conf.5"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "cpu-models.conf(5)"
---
[â](#)

## NAME

cpu-models.conf - Custom CPU model configuration file

## SYNOPSIS

/etc/pve/virtual-guest/cpu-models.conf

## DESCRIPTION

The /etc/pve/virtual-guest/cpu-models.conf file stores custom CPU
models, which can be used by VMs to get access to advanced CPU
features (for example custom CPU flags).

## File Format

CPU models each have their own section in the file, beginning with
the header:

```bash
cpu-model: <name>
```

Note that <name> does not include the custom- prefix, which is
required in VM configs to denote custom CPU models.

For example, if the <name> is foobar, the CPU for a VM would need to be
configured as custom-foobar.

Each section can specify several options. They are indented by either one TAB
character or multiple spaces. Every option and its value is separated by one
space, for example:

```bash
reported-model qemu64
```

See below for all available options.

Blank lines and those starting with a # are ignored.

## Options

flags: <+FLAG[;-FLAG...]>

List of additional CPU flags separated by ;. Use +FLAG to enable, -FLAG to disable a flag. There is a special nested-virt shorthand which controls nested virtualization for the current CPU (svm for AMD and vmx for Intel). Custom CPU models can specify any flag supported by QEMU/KVM, VM-specific flags must be from the following set for security reasons: aes, amd-no-ssb, amd-ssbd, hv-evmcs, hv-tlbflush, ibpb, md-clear, nested-virt, pcid, pdpe1gb, spec-ctrl, ssbd, virt-ssbd

guest-phys-bits: <integer> (32 - 64)

Number of physical address bits available to the guest.

hidden: <boolean> (default = 0)

Do not identify as a KVM virtual machine. Only affects vCPUs with x86-64 architecture.

hv-vendor-id: <vendor-id>

The Hyper-V vendor ID. Some drivers or programs inside Windows guests need a specific ID.

level: <integer> (0 - 4294967295)

Maximum input value for the basic CPUID leaves the guest can query - that is the vendor (leaf 0), family/model/stepping and feature bits (leaf 1), cache and topology info (leaves 4 and B), and so on. Higher-numbered leaves are hidden. Setting 30 is a common workaround for Hyper-V boot failures on Windows guests running on recent Intel hosts. Only applies when the vCPU architecture is x86_64.

phys-bits: <8-64|host>

The physical memory address bits that are reported to the guest OS. Should be smaller or equal to the host’s. Set to host to use value from host CPU, but note that doing so will break live migration to CPUs with other values.

reported-model: <486 | Broadwell | Broadwell-IBRS | Broadwell-noTSX | Broadwell-noTSX-IBRS | Cascadelake-Server | Cascadelake-Server-noTSX | Cascadelake-Server-v2 | Cascadelake-Server-v4 | Cascadelake-Server-v5 | ClearwaterForest | ClearwaterForest-v2 | ClearwaterForest-v3 | Conroe | Cooperlake | Cooperlake-v2 | DiamondRapids | EPYC | EPYC-Genoa | EPYC-Genoa-v2 | EPYC-IBPB | EPYC-Milan | EPYC-Milan-v2 | EPYC-Milan-v3 | EPYC-Rome | EPYC-Rome-v2 | EPYC-Rome-v3 | EPYC-Rome-v4 | EPYC-Rome-v5 | EPYC-Turin | EPYC-v3 | EPYC-v4 | EPYC-v5 | GraniteRapids | GraniteRapids-v2 | GraniteRapids-v3 | GraniteRapids-v4 | GraniteRapids-v5 | Haswell | Haswell-IBRS | Haswell-noTSX | Haswell-noTSX-IBRS | Icelake-Client | Icelake-Client-noTSX | Icelake-Server | Icelake-Server-noTSX | Icelake-Server-v3 | Icelake-Server-v4 | Icelake-Server-v5 | Icelake-Server-v6 | Icelake-Server-v7 | IvyBridge | IvyBridge-IBRS | KnightsMill | Nehalem | Nehalem-IBRS | Opteron_G1 | Opteron_G2 | Opteron_G3 | Opteron_G4 | Opteron_G5 | Penryn | SandyBridge | SandyBridge-IBRS | SapphireRapids | SapphireRapids-v2 | SapphireRapids-v3 | SapphireRapids-v4 | SapphireRapids-v5 | SapphireRapids-v6 | SierraForest | SierraForest-v2 | SierraForest-v3 | SierraForest-v4 | SierraForest-v5 | Skylake-Client | Skylake-Client-IBRS | Skylake-Client-noTSX-IBRS | Skylake-Client-v4 | Skylake-Server | Skylake-Server-IBRS | Skylake-Server-noTSX-IBRS | Skylake-Server-v4 | Skylake-Server-v5 | Westmere | Westmere-IBRS | a64fx | athlon | core2duo | coreduo | cortex-a35 | cortex-a53 | cortex-a55 | cortex-a57 | cortex-a710 | cortex-a72 | cortex-a76 | cortex-a78ae | host | kvm32 | kvm64 | max | neoverse-n1 | neoverse-n2 | neoverse-v1 | pentium | pentium2 | pentium3 | phenom | qemu32 | qemu64> (default = kvm64)

CPU model and vendor to report to the guest. Must be a QEMU/KVM supported model. Only valid for custom CPU model definitions, default models will always report themselves to the guest OS.

## Example File

```bash
cpu-model: avx
    flags +avx;+avx2
    phys-bits host
    hidden 0
    hv-vendor-id proxmox
    reported-model kvm64
```

## Copyright and Disclaimer

Copyright © 2007-2022 Proxmox Server Solutions GmbH

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public
License along with this program.  If not, see
[https://www.gnu.org/licenses/](https://www.gnu.org/licenses/)