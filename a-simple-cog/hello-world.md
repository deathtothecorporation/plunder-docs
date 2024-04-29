---
description: 'Document Type: Tutorial'
---
# "Hello World" Cog

We're finally ready to see a simple cog. This file would be called
`sire/test_cog.sire`:

```test_cog.sire
#### test_cog <- prelude

/+  prelude

;;;;;

= (helloWorld return)
| trk "hello world"
| return ()

main=(runCog helloWorld)
```

# Running the Cog

Before we inspect the code, let's run it and see something happen!  

First, create the file `sire/test_cog.sire` with the above contents and save it.

Now, choose a directory where you want your plunder ships to get created. We'll use a
`my_ships` directory for the examples here. We'll proclaim that the name of the
ship that contains this cog will be "hello_world". Thus, the command to boot
this ship and initiate this cog is:

```
plunder boot ~/my_ships/hello_world sire/test_cog.sire
```

When you run this command, you'll see a whole bunch of output, with something
like this near the end:

```
<...>
("datom","LOADED FROM CACHE!")
("prelude","LOADED FROM CACHE!")
("test_cog","LOADED FROM CACHE!")
(helloWorld a)=(_Trace:{hello world} a-0)

{hello world} ;; <-- Here's your trk-logged message

main=(KERNEL [0 0] 0 [0] [0])

("cache hash","7y5a286DrMFXWwSJBiPAXYnpoYjqEfJcvSJuiBw1GKV4")
```

Congratulations, you've booted a plunder ship that runs a single cog which logs a string to the console.

# Understanding the Cog

Now let's go through it line-by-line to get a high-level understanding.

## Imports

We don't want to get too far into the weeds just yet with includes, boot order
and library imports, but we also don't want you to be puzzled by too many
unexplained lines of code.

```sire
#### test_cog <- prelude
```

This has to do with includes and load order. `test_cog.sire` is the name of the
current file, and `prelude.sire` is the name of a dependency. the `#### foo <-
bar` syntax says something like "Take `bar` and put it before `foo` in the load
order, and then load `foo`".

```sire
/+  prelude
```

The `/+` rune imports a module. In this case, `prelude.sire` just does a bunch of
other importing of kernel code, convenience functions, syscalls, and basically
everything else we need to run a cog.

```sire
;;;;;
```

This is just a comment line dividing the imports from the business code

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
argument `"hello world"`.

```sire
| return ()
```

And it invokes the `return` argument that it received, sort of like a
callback. Don't worry too much about this bit for now.

## `main` and `runCog`

Finally, we have this line in the `test_cog.sire` file:

```
main=(runCog helloWorld)
```

Think of this as boilerplate. Every cog should end in a
`main=(runCog someNameHere)` line which can be thought of as ultimately kicking
off the `someNameHere` process previously-defined.
