---
description: 'Document Type: Explanation'
---

# A device, a payment system, an app store

This document presents an overview of the system being developed at the Vaporware project. The sections following it provide a more detailed explanation of:

* The problems this system attempts to solve and how it approaches those problems
* The core technical components, including the Plunder Virtual Machine and PLAN data model

The Vaporware project consists of three closely related systems: a purely functional virtual machine, a monetized application / software package registry, and an app store / package management program. The system is being developed as Free and Open Source Software and respects the [four essential freedoms](https://www.gnu.org/philosophy/free-sw.en.html).

## A device

The Vaporware project is attempting to commercialize a new device-type category called a **Solid-State Interpreter** (SSI). A solid-state interpreter is a stateless function, defined via a minimal set of axiomatic combinators, which implements an [ACID](https://en.wikipedia.org/wiki/ACID) database. The system is purely functional and Turing complete. It is a single-level store and makes no distinction between memory and disk. The interpreter operates over an Event Log in a similar manner to a blockchain, but does not guarantee strict global ordering of events.

SSIs are architecturally similar to other combinator systems, like the SECD machine, but are even more minimal. Programmers familiar with LISP or its dialects will find many similarities between the two environments.

Vaporware has selected the Plunder Virtual Machine and PLAN execution model as our specific SSI implementation. Although not a perfect analogy, one can conceive of the PlunderVM as a minimal framework for building purely functional unikernels, with the PLAN data model as a highly portable intermediate representation. Plunder was selected for its high performance, stable hardware interface, and backward compatibility with existing languages. Comparing Plunder to a functional WASM is not far off the mark.

On top of this framework and within the PlunderVM, Vaporware will bundle together web server functionality, decentralized and open identity systems, a peer-to-peer networking protocol, user session management and authentication, and integration with a number of external services. The goal of this device is to deliver the following features:

* Full-stack live-reload for local development
* Orthogonal persistence
* Integrated networking / interchange
* Type-safe migrations
* Hot-reload deployments
* Encrypted p2p networking
* Support for multi-scale and multi-cloud

While these properties are generally desirable, the Vaporware distribution of Plunder is targeted at end users - that is, normal people. We believe that every computer user should have access to the cloud without incurring the costs of monopoly rent extraction. It's our hope that the stability, simplicity, portability, and low maintenance costs of the solid-state interpreter can make this vision real.

### A payment system

{% hint style="warning" %}
TODO
{% endhint %}


### The Plunder VM

{% hint style="warning" %}
TODO: seems worth adding a super brief snippet of what the plunder VM is **generally about** and why one would care to use it.

Below is cribbed right from the plunder sourcehut:
{% endhint %}

> Hardware restarts are invisible to the software, as is moving a running program from one physical machine to another.

> Because of this, programs don't need to save their state into an external database, they can simply keep everything "in memory", and that state will be synchronized to disk transparently

----

If you're interested in experimenting right away, head over to [Getting plunder installed on your machine](setup/installation.md). If you'd prefer to learn more about the system before handling it, continue on below:
