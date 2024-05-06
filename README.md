# Introduction

## Vaporware

Vaporware is a distributed operating system designed for use by individual people, not corporations. It is our belief that a personal internet, composed of computers entirely owned by normal people, is both possible and desirable. Our project owes its existence to the Urbit community—both for inspiration and support—but shares none of Urbit’s codebase and diverges from many of the principles and technical choices of the Urbit project.

Vaporware runs on the Plunder virtual machine. [Plunder is a solid-state interpreter](https://git.sr.ht/\~plan/plunder): a programming environment which combines orthogonal persistence, functional programming, and homoiconicity. Imagine a Scheme Lisp machine with automatically persisted application state, and you’re not far off the mark. Plunder and Vaporware are not the same project, but share some of the same long term goals; Vaporware is the first third-party developer to adopt the Plunder software stack.

This documentation introduces the Plunder system:

* **PLAN:** the data model and graph reduction system of Plunder
* **Sire:** a functional language, bootstrapped from PLAN
* **Cogs:** a microservices architecture for userspace application development

This documentation is incomplete and should be supplemented with the primary Plunder repo.

## Document Goals

The goal of this documentation is to get you from "I have no idea what this system is" to a simple "Hello world" web app.

The plan for this not-so-quick-start is:

1. Get familiar with enough of the Sire language to be immediately useful
2. Gather enough context about how IO and Cogs work to have a decent mental model of what's going on when you build one
3. Write a simple Cog from scratch

We'll start off here with a very high-level overview of what Plunder is and how it works at this point in time. There will be links out to deeper dives on some topics, but we recommend you first go through the main path of these docs before you deepen your understanding of any given core concept.\


***

If you're interested in experimenting right away, head over to [Getting plunder installed on your machine](setup/installation.md). If you'd prefer to learn more about the system before handling it, continue on below:
