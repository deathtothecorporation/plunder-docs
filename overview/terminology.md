---
description: 'Document Type: Explanation'
---

# Basic Terminology

## Ships

A Plunder Virtual Machine is colloquially referred to as a "Ship". On the host filesystem, a ship consists of a directory containing a `data.mdb`, `lock.mdb` and a `pins` directory:

```
$ tree my-ships/ship1
ship1
├── data.mdb
├── lock.mdb
└── pins
    ├── 24
    │   └── 24ZTpfkWiejf34UAL21RNUWtTwz2sckvBQqYj4Yn1d4f
    ├── 2B
    │   └── 2BHstS7roP2kPE34XRUweJM9ZtBRcpG3wEdxP5ALhxSN
    <etc>
```

## Cogs

A "Cog" is a is a persistent process running on a ship. Cogs snapshot state and write inputs to an event log. They recover state after a restart by loading a recent snapshot and re-applying inputs. Cogs interact with the world by making system calls - which are included as part of their state (thus a cog's set of system calls also resume after a restart).

---

Read on to learn about the PLAN evaluation model and the Sire language - the fundamental physics of Ships and Cogs:
