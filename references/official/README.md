---
title: Server Setup Docs — Official References Library
tags: [server-setup, references, official-documentation, obsidian, ai-index]
---

# Server Setup Docs — Official References Library

> [!success] Complete Offline Knowledge Vault
> Synchronized, localized Markdown documentation collection built specifically for human navigation and AI agent retrieval.

## 📚 Official Documentation Repositories

| Repository | Focus & Architecture Role | Pages | Size | Primary Indexes |
|---|---|---:|---:|---|
| **NATS Server & JetStream** | Pub/Sub Messaging, Distributed Queues & KV | 186 | 1.93 MB | [[nats-docs/README\|README]] · [[nats-docs/AI_INDEX\|AI Index]] |
| **FoundationDB** | Distributed Transactional Core Database | 110 | 1.24 MB | [[foundationdb-docs/README\|README]] · [[foundationdb-docs/AI_INDEX\|AI Index]] |
| **Stalwart Mail Server** | Async MTA, IMAP4, JMAP & Spam Filtering | 477 | 3.25 MB | [[stalwart-docs/README\|README]] · [[stalwart-docs/AI_INDEX\|AI Index]] |
| **MinIO Object Storage** | S3-Compatible Storage Engine (via Silo) | 411 | 3.61 MB | [[minio-docs/README\|README]] · [[minio-docs/AI_INDEX\|AI Index]] |
| **Redis Server** | In-Memory Data Store, Caching & Failover | 511 | 3.64 MB | [[redis-docs/README\|README]] · [[redis-docs/AI_INDEX\|AI Index]] |
| **Nginx Web Server** | Ingress Controller, Reverse Proxy & SSL Gateway | 126 | 1.42 MB | [[nginx-docs/README\|README]] · [[nginx-docs/AI_INDEX\|AI Index]] |
| **HAProxy Load Balancer** | Layer 4 & Layer 7 High-Availability Proxy | 6 | 3.49 MB | [[haproxy-docs/README\|README]] · [[haproxy-docs/AI_INDEX\|AI Index]] |
| **Proxmox VE** | Hypervisor & LXC Container Administration | 51 | 2.87 MB | [[proxmox-docs/README\|README]] · [[proxmox-docs/AI_INDEX\|AI Index]] |
| **Debian System Manuals** | Core Linux OS, Sysadmin, Networking & Security | 237 | 2.90 MB | [[debian-docs/README\|README]] · [[debian-docs/AI_INDEX\|AI Index]] |
| **HPE ProLiant Gen10** | iLO 5, UEFI BIOS & Smart Array Storage | 4 | 8.25 MB | [[hpe/README\|README]] · [[hpe/HPE_AI_CHEAT_SHEET\|HPE Cheatsheet]] |

---

## 🤖 AI & RAG Agent Guidelines

1. **Routing Strategy:** Always query `AI_INDEX.md` inside each target documentation folder before loading individual Markdown notes.
2. **Link Integrity:** All internal documentation links are converted to relative Obsidian Wikilinks.
3. **Machine Metadata:** Every repository includes a `manifest.json` mapping URLs, file paths, titles, and exact byte counts.
