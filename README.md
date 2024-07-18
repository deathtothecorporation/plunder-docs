{% hint style="warning" %}
🚧 This documentation collection is under active development. Similar callouts will be used to denote areas that are stubbed-out or coming soon. 🚧
{% endhint %}

# Introduction

_This documentation is best viewed at [https://vaporware.gitbook.io/pallas](https://vaporware.gitbook.io/pallas)_.

## Pallas

The Pallas [SSI](https://wiki.vaporware.network/solid-state%20interpreter) programming environment is written in a purely functional, rune-based language called Sire. Sire is a sort of Lispy-Haskell with a visual resemblance to Hoon.

This documentation introduces the Pallas system:

* **PLAN:** the data model and graph reduction system of Pallas
* **Sire:** a functional language, bootstrapped from PLAN
* **Cogs:** a microservices architecture for userspace application development

## Document Goals

The goal of this documentation is to get you from "I have no idea what this system is" to a simple "Hello world" web app.

The plan for this not-so-quick-start is:

1. Get familiar with enough of the Sire language to be immediately useful
2. Gather enough context about how IO and Cogs work to have a decent mental model of what's going on when you build one
3. Write a simple Cog from scratch

We'll start off here with a very high-level overview of what Pallas is and how it works at this point in time. There will be links out to deeper dives on some topics, but we recommend you first go through the main path of these docs before you deepen your understanding of any given core concept. If you're viewing on the web, use the "Next" button at the bottom of each page. If you're looking directly at the source files, `SUMMARY.md`, followed top-to-bottom provides the same path.

## Resources

- [Pallas repo](https://github.com/operating-function/pallas)
- The [Vaporware website](https://vaporware.network)
- [Pallas developer Telegram group](https://t.me/vaporwareNetwork)

---

If you're interested in experimenting right away, head over to [Getting pallas installed on your machine](setup/installation.md). If you'd prefer to learn more about the system before handling it, continue on below:
