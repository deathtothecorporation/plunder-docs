# Introduction

{% hint style="warning" %}
🚧 This documentation collection is under active development. Similar callouts will be used to denote areas that are stubbed-out or coming soon. 🚧
{% endhint %}

## Introduction

Pallas is an event sourced, purely functional operating system, called an _**operating function**_**.**&#x20;

Pallas provides an entirely unique set of features, but is inspired by a long history of systems and language research, including [MIT’s exokernel](https://pdos.csail.mit.edu/archive/exo/). Pallas collapses the distinction between database, virtual machine, and language platform, creating a stable, simple, and extensible application environment.

The cost to this design is a clean break with legacy systems, including Unix and most popular programming languages. In return, Pallas offers a set of features that are found nowhere else:

* All application code is automatically persisted, without imports or boilerplate. To create a database, you write a pure function. System calls are included in persisted state.
* Partially applied functions can be serialized and stored on-disk, or sent over the wire. Programs in mid-execution can be paused, moved to a new machine, and resumed with no impact.
* Program execution can be parallelized via a process model. A single machine can spawn and manage multiple processes.
* The system’s default language blends features from both Lisp and Haskell. It uses a more readable and flexible syntax than S-expressions, without sacrificing regularity or metaprogramming capabilities. The system supports hot reload, macro-based type systems, all the way up to custom compilers.
* Data and code is deduplicated, merkleized, and stored in content-addressable memory pages. This creates a global referentially-transparent content store, which is naturally complemented by protocols like BitTorrent.

By reading our documentation and examples, you will be able to prove to yourself that these claims are true.

The foundation of Pallas is untyped, but conceptually we can say that a database is a function of the type:

```haskell
type DB = Input -> (Output, DB)
```

If a user supplies such a function, the Pallas runtime will create a database using a snapshot-and-event-log system. The user can write their programs as if they were keeping their data "in memory", without any need for manual persistence or other forms of cache management.

The recursive part of the type above might seem strange. You can think of it almost as a normal stateful function:

```haskell
type OtherDB = (State, Input) -> (State, Output)
```

The difference is that instead of changing the state value, the recursive version would change itself. The current version of the function is the state. In other words: programs can upgrade themselves dynamically. Code can construct code. Because of this, we can put an entire compiler toolchain inside the system and the programs it generates have zero dependencies on the outside world.\


This project is a fork of [Plunder](https://sr.ht/\~plan/plunder/).

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
* [Pallas developer Telegram group](https://t.me/vaporwareNetwork)

***

If you're interested in experimenting right away, head over to [Getting Pallas installed on your machine](setup/installation.md).
