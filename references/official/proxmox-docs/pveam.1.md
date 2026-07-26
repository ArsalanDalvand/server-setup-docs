---
title: "pveam(1)"
source: "https://pve.proxmox.com/pve-docs/pveam.1.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pveam.1"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pveam(1)"
---
[â](#)

## NAME

pveam - Proxmox VE Appliance Manager

## SYNOPSIS

pveam <COMMAND> [ARGS] [OPTIONS] [](#cli_pveam)

pveam available [OPTIONS] [](#cli_pveam_available)

List available templates.

--section <mail | system | turnkeylinux>

Restrict list to specified section.

pveam download <storage> <template> [](#cli_pveam_download)

Download appliance templates.

<storage>: <storage ID>

The storage where the template will be stored

<template>: <string>

The template which will downloaded

pveam help [OPTIONS] [](#cli_pveam_help)

Get help about specified command.

--extra-args <array>

Shows help for a specific command

--verbose <boolean>

Verbose output format.

pveam list <storage> [](#cli_pveam_list)

Get list of all templates on storage

<storage>: <storage ID>

Only list templates on specified storage

pveam remove <template_path> [](#cli_pveam_remove)

Remove a template.

<template_path>: <string>

The template to remove.

pveam update [](#cli_pveam_update)

Update Container Template Database.

## DESCRIPTION

Command line tool to manage container images. See man pct for usage
examples.

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