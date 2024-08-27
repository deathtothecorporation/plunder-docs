# Introduction

{% hint style="warning" %}
🚧 This documentation collection is under active development. Similar callouts will be used to denote areas that are stubbed-out or coming soon. 🚧
{% endhint %}

Pallas is an event sourced, purely functional application platform, called an _**operating function**_. Every operation inside an operating function is ACID. Pallas is currently instantiated as a VM and can be run on Linux, Mac, or Nix. The platform ships with a minimal bootstrapping language called _Sire_, but includes an efficient axiomatic IR which can be targeted by mainstream functional languages.

### Problem

_Software is being destroyed by accidental complexity_. Our most popular applications require hundreds or thousands of developers to build and maintain. This complexity benefits large corporations who use their dominant capital positions to monopolize and rent seek. This dynamic increases the cost of software, reduces the set of economically viable programs, and disempowers developers and users alike.

This is not an abstract moral problem. Software is the best tool we have for identifying, organizing, and solving societal issues. Instability in software trickles down to every other domain of human activity.

### Solution

More composable software systems directly result in more software freedom. The more power individual developers wield, the more that power is widely distributed. This is true even if "scrolling through silos" is the highest state of computing.

To increase developer power and software freedom, your entire computer needs to be inspectable and understandable. It should be composable and made of highly generic, unopinionated, easily understood, reusable building blocks. These properties need to be stable regardless of underlying hardware changes and should guarantee a high degree of backward and forward compatibility.

Pallas is a prototype of such a system.

### Features

* **No database code**
  * All application data is automatically persisted, without the need for imports or boilerplate. To create a database, you write a pure function.
* **Serialize anything, running programs included**
  * Closures can be serialized and stored on-disk, or sent over the wire. Programs in mid-execution can be paused, moved to a new machine, and resumed with no impact. Open syscalls are included in persisted state and are resumed on reboot.
* **Parallelism with deterministic replay**
  * Results from spawned processes, IO, and runtime evaluated expressions are recorded as events using an event-log-and-snapshot model. On replay, terminated events are recomputed with perfect determinism.
* **Global referentially-transparent content store**
  * Data and code is deduplicated, merkleized, and stored in content-addressable memory pages. This creates a global referentially-transparent content store, which is naturally complemented by protocols like BitTorrent.
* **Native networking and identity**
  * VMs and spawned processes are identified by one or more cryptographic keys. The networking protocol is stateless and guarantees at-least-once-delivery. The runtime implements the protocol, allowing transport details to evolve without breaking internal software.
* **Formally specified system calls**
  * Software breaks at boundaries, so syscalls are specified as pure functions and their spec is designed to be frozen. Runtimes are responsible for matching... (_TODO: enforcing semantics?_)
* **Extensible language platform**
  * Metaprogramming capabilities include hot reload, zero-overhead virtualization, macro-based type systems, all the way up to custom compilers.

### System Overview

The foundation of Pallas is untyped, but conceptually we can say that a process is database. Each database is a function of the type

```haskell
type DB = Input -> (Output, DB)
```

If a user supplies such a function, the Pallas runtime will create a database using a snapshot-and-event-log system. The user can write their programs as if they were keeping their data "in memory", without any need for manual persistence or other forms of cache management.

The recursive part of the type above might seem strange. You can think of it almost as a normal stateful function:

```haskell
type OtherDB = (State, Input) -> (State, Output)
```

The difference is that instead of changing the state value, the recursive version would change itself. The current version of the function is the state. In other words: programs can upgrade themselves dynamically. Code can construct code. Because of this, we can put an entire compiler toolchain inside the system and the programs it generates have zero dependencies on the outside world.

### Pallas

This documentation introduces the Pallas system:

* **PLAN:** the data model and graph reduction system of Pallas
* **Sire:** a functional language, bootstrapped from PLAN
* **Cogs:** a microservices architecture for userspace application development

### Document Goals

The goal of this documentation is to get you from "I have no idea what this system is" to a simple "Hello world" web app.

The plan for this quick-start is:

1. Get familiar with a minimally useful amount of Sire.
2. Develop intuition around IO and Cogs.
3. Write a simple Cog from scratch.

We'll start off here with a high-level overview of Pallas. If you're viewing on the web, use the "Next" button at the bottom of each page. If you're looking directly at the source files, `SUMMARY.md`, followed top-to-bottom provides the same path.

### Resources

* [Pallas repo](https://github.com/operating-function/pallas)
* [Telegram Support](https://t.me/vaporwareNetwork)

***

If you're interested in experimenting right away, head over to [Getting Pallas installed on your machine](setup/installation.md).
