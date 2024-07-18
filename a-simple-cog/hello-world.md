---
description: 'Document Type: Tutorial'
---

# "Hello World" Cog

You're finally going to start writing code in `.sire` files that you boot and run, rather than directly into the REPL. A few things to note on that: You can't have blank new lines within the body of a function, but comments (lines that start with `;`) are fine. Indentation matters, but you'll start to see that in practice - we won't explain it up front.

_**FINALLY!**_

```sire
#### hello_world_cog <- prelude

:|  prelude

;;;;;

= (helloWorld return)
| trk "hello world"
| return ()

main=(runCog helloWorld)
```

# Running the Cog

Before we inspect the code, let's run it and see something happen!  

First, create the file `sire/hello_world_cog.sire` (within the pallas repo, right next to all the other `sire/*.sire` files) with the above contents and save it.

Now, choose a directory where you want your pallas ships/VMs to get created. We'll use a
`my_ships` directory for the examples here. We'll proclaim that the name of the
ship (the directory name) that contains this cog will be `hello_world`. Thus, the command to boot
this ship and initiate this cog is (you remembered to `nix develop` right?):

```
pallas boot ~/my_ships/hello_world sire/hello_world_cog.sire
```

When you run this command, you'll see a whole bunch of output, with something
like this near the end:

```
<...>
("datom","LOADED FROM CACHE!")
("prelude","LOADED FROM CACHE!")
("hello_world_cog","LOADED FROM CACHE!")
(helloWorld a)=(_Trace:{hello world} a-0)

{hello world} ;; <-- Here's your trk-logged message

main=(KERNEL [0 0] 0 [0] [0])

("cache hash","7y5a286DrMFXWwSJBiPAXYnpoYjqEfJcvSJuiBw1GKV4")
```

Congratulations, you've booted a pallas ship that runs a single cog which logs a string to the terminal.

# Understanding the Cog

Now let's go through it line-by-line to get a high-level understanding.

## Imports

We don't want to get too far into the weeds just yet with includes, boot order
and library imports, but we also don't want you to be puzzled by too many
unexplained lines of code.

```sire
#### hello_world_cog <- prelude
```

This has to do with includes and load order. `hello_world_cog.sire` is the name of the current file, and `prelude.sire` is the name of a dependency. the `#### foo <- bar` syntax says something like "Take `bar` and put it before `foo` in the load order, and then load `foo`".

```sire
:|  prelude
```

The `:|` rune imports a module. In this case, `prelude.sire` just does a bunch of
other importing of kernel code, convenience functions, syscalls, and basically
everything else we need to run a cog.

```sire
;;;;;
```

This is just a comment line dividing the imports from the business code. lol.

## Binding the function

```sire
= (helloWorld return)
```

`=` defines a top-level binding for a function named `helloWorld` which takes a
single argument named `return`.

This function only does a couple things:

```sire
| trk "hello world"
```

It applies the `trk` function (which you'll recall logs to the console) to the
argument `"hello world"`, a string.

```sire
| return ()
```

And it applies the `return` argument that it received, sort of like a
callback. Don't worry too much about this bit for now.

## `main` and `runCog`

Finally, we have this line:

```
main=(runCog helloWorld)
```

Think of this as boilerplate. Every cog should end in a `main=(runCog someNameHere)` line which can be thought of as ultimately kicking off the `someNameHere` process previously-defined.
