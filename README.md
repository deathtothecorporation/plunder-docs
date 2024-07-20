# Introduction

{% hint style="warning" %}
🚧 This documentation collection is under active development. Similar callouts will be used to denote areas that are stubbed-out or coming soon. 🚧
{% endhint %}

## Introduction

Pallas is a purely functional exokernel and library OS, designed to radically simplify the modern networked computing stack. We call the libOS an "operating function" because it is defined as a pure function of its event input stream.

We hypothesize that by extending typed functional programming to the entire OS environment, small teams will be able to outcompete large technology companies. In particular, we believe this innovation will enable us to dissolve application boundaries, simplify distributed systems programming, eliminate large categories of devops, and dramatically expand the domain of end-user programming.

Key system properties:

* merkleized state and content-addressable memory pages
* native networking with public keys as endpoints
* serializable closures that can be transferred over the network
* traditional Lisp-style metaprogramming (`defmacro` + syntax-rules)
* automatic durable execution (orthogonal persistence)

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

If you're interested in experimenting right away, head over to [Getting Pallas installed on your machine](setup/installation.md).&#x20;
