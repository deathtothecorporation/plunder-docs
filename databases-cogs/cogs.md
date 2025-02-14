# Introduction

{% hint style="warning" %}
This page is under active development.
{% endhint %}

### Machines

In the Pallas VM, a "machine" is a set of persistent processes, which are referred to as "cogs".

A user creates a cog by writing a transition function of a particular shape and giving that to the runtime. The details around getting the transition function into this "particular shape" are easily handled by a small library.

Cogs survive restarts. The runtime writes their inputs to an event log, and occasionally snapshots their states. On restart, the runtime recovers the most recent state of a cog by loading its most recent snapshot and then replaying all inputs after that point. Because of this built-in persistence strategy, you can view a cog as a database.

Cogs interact with the world by making system calls. The set of active system calls made by a cog is part of its current formal state. When a cog is reloaded, the runtime will resume any active calls as well.

There is no hidden state in a machine. A machine can shut down and resume without any visible effect. The formal state of a full Machine is a PLAN value of shape `Tab Nat PLAN`: a mapping from IDs to cogs.

### Cogs

More formally, a cog is just a PLAN value (a partially applied law/function, to be specific). In order to be a valid cog, the PLAN value must contain at least the following two values as part of its state:

* An array of worker processes that handle IO and concurrent evaluation. Like everything else in Pallas, these are also functions encoded as PLAN values.
* An arbitrary state value and an accompanying function which the worker processes can use to query the cog in a read-only way. Since it’s read-only, this can be trivially parallelized: there are no race conditions. Many workers can query the cog simultaneously.

The cog never interacts with the outside world at all, it only spawns worker processes by putting them in its array of workers. These workers will then open connections to the outside world, and submit write requests to the cog.&#x20;

For now, the only interfaces that workers have to the outside world are TCP and UDP. Since you can do nearly everything over these we don’t have to continuously add IO interfaces to the standard. This narrow waist design is one of the architectural decisions that makes the core Pallas system freezable (e.g. the set of all IO interfaces can actually be completed).&#x20;

For example, this means our HTTP server is just a worker process which communicates over TCP, instead of a special kernel module which has corresponding code in the runtime. By limiting ourselves to TCP+UDP we can do everything within the system, instead of having to extend the runtime. A core goal is to keep the runtime as minimal as possible, so that it's easy to write a new one. You shouldn't have to trust the runtime maintainers.

Workers have their own states, so they can engage in stateful protocols, such as sessions and handshakes, but this state is transient. If a worker crashes, or if the physical machine turns off, its state will disappear. But since the array of workers is part of the cog’s state, they can be restarted immediately. They will have lost their own states, but they should’ve submitted anything important to the cog anyway. That’s what they are there for: to filter and assemble incoming data, and decide when something is worth persisting. This allows us to avoid building an additional exoskeleton in the runtime which decides when something should hit the event log or not. Those choices are all formally specified, completely inside the system.

### From the Old Docs

{% hint style="warning" %}
_The following is conceptually right but details are wrong. It should be updated and edited._
{% endhint %}

A cog is a Pallas function partially applied to a row of syscalls.

```
($cog [[%eval 0 add 2 3] [%rand 0 %byte 8]])
```

An event is given to a cog by calling it as a function. Each event is a set of syscall responses `(Map Nat Any)`:

```
| ($cog [[%eval 10 add 2 3] [%rand 0 %byte 8]])
| %% =0 [5]
| %% =1 x#0011223344556677
```

There are four types of syscalls: `%eval` requests, `%cog` requests, `%what` requests to detect the attached hardware and everything else is treated as a CALL to hardware. The interpreter ignores any value that it does not understand.

A CALL to hardware looks like `[%rand 0 %byte 8]`. Breaking it down:

* `%rand` is the name of the hardware targeted.
* The `0` is the "durability" flag that tells the ships that the effect should not be executed until the input that lead to the effect has been committed to the log.
* `[%byte 8]` is the argument-list that is passed to the `%rand` device.

If the current Pallas VM does not have a piece of hardware given the passed in name, no attempt to handle the request will be made.

That's why we need `%what` to synchronize what the cog thinks are the callable pieces of hardware. A cog doesn't detect when the Pallas interpreter has been restarted or replaced with a different one and must know about what current capabilities are provided by CALL. At first, a cog issues a `[%what %[]]` request, receives a `%[%rand %http]` response and then holds open a `[%what %[%rand %http]]` request which will only change if the interpreter does.

`%eval` asks for Pallas code to be evaluated asynchronously. The result is that we can take advantage of parallelism, and that the main loop is not slowed down when the Cog needs to perform an expensive computation.

* The `10` in `[%eval 10 add 2 3]` is an upper-bound on the number of seconds that an evaluation is allowed to run for. An evaluation that takes longer than that is canceled.
* The `[add 2 3]` indicates that EVAL should evaluate the expression (add 2 3).
* The reason that `%eval` is special, is because the event log does not actually contain the result of an EVAL call, instead the event log simply records that the event succeeded, and the result is re-calculated on replay.
* This is important because it means that extremely large values can be returned by EVAL without bogging down the log.

Finally, there are the `%cog` requests. A user is likely to have multiple processes that they wish to run, and having those processes communicate over hardware CALLs would mean that each IPC message must be written into the event log. So we have a few special calls for process management and IPC between cogs. Like `%eval`, most `%cog` requests have special event log representations so that you're storing a record that something happened that could be recalculated on log replay.

(If cog A sends a message to cog B, all you need to do is record that B processed the message from cog A at a given request index, instead of serializing and storing the full noun sent in the event log.)

The `%cog` requests are:

* `[%cog %spin fan] -> IO Pid`: Starts a cog and returns its cog id.
*   `[%cog %ask pid chan fan] -> IO (Maybe Fan)`: Sends the fan value to pid on a channel, returning afterwards. `%ask`/`%tell` operate on Word64 channels which allows a cog to offer more than one port or "service". `%ask` makes a request of a different cog which has an open `%tell` request.

    Returns `0` on any error (remote cog doesn't exist, remote cog's `%tell` function crashed), or the Just value (`0-result`) on success.
*   `[%cog %tell chan fun] -> IO a`: Given a function with a type `> CogId > Any > [Any a]`, waits on the channel `chan` for a corresponding `%ask`. The runtime will atomically match one `%ask` with one `%tell`, and will run the tell function with the ask value. The output must be a row, and the row's index-zero value will be sent back to the `%ask`, and the row's index-one value will be sent back to the `%tell`.

    Execution and response is atomic; you'll never have one without the other in the written event log. This operation is used to allow two different threads to act in concert.

    Execution and response are atomic; each response map that contains an %ask or %tell will _only_ contain an %ask or %tell. Unlike all other responses, the runtime will not put as many responses as possible in the event which delivers an %ask or a %tell response.

    Any crash while evaluating `fun` with the arguments will count as crashing the telling cog.
* `[%cog %stop pid] -> IO (Maybe CogState)`: If the cog does not exist, immediately returns None. Otherwise, stops and removes the cog from the set of cogs and returns the `CogState` value.
*   `[%cog %reap pid] -> IO (Maybe CogState)`: If the cog does not exist, immediately returns None. Otherwise, waits for a cog to enter an error state, removes the cog from the set of cogs and returns the `CogState`.

    (In the case where there's a %reap and a %stop open, the calling `%stop` takes precedent and receives the cog value, and the `%reap` receives None.)
*   `[%cog %wait pid] -> IO ()`: If the cog is not running (non-existent, finished, crashed or timed out), immediately return 0. Otherwise, wait for the cog to no longer be in the running state and return 0.

    Separate from %reap and %stop, cogs need a way to detect that cogs do not exist even when they aren't responsible for stopping or cleaning up after a crash.
* `[%cog %who] -> IO Pid`: Tells the cog who it is. Any other way of implementing this would end up with changes to the type of the cog function taking an extra `Pid ->`.

The on disk snapshot of a whole Machine is just the noun value of `(Tab Pid CogState)` serialized, where Pid is a natural number and `CogState` is a row matching one of the following patterns:

* `[0 fan]`: represents a spinning cog which has requests and can process responses.
* `[1 fan]`: represents a finished cog, a cog which shut down cleanly by having no requests, so it will never receive a response in the future.
* `[2 (op : nat) (arg : fan) (final : fan)]`: represents a crashed cog, with the `op` and `arg` being the values that caused the crash and `final` being the final value of the cog before the crashing event.
* `[3 (duration : nat) (final : fan)]`: represents a cog which had a request timeout.

These patterns are also what are returned in the `[%cog %stop]` and `[%cog %reap]` requests.
