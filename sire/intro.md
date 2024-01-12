# Intro to Sire

As already discussed, the _point_ of Sire is to get back PLAN.
In the near future, mainstream functional languages - like
[Elm](https://elm-lang.org/) - will compile to PLAN. At that point, much Plunder
development - including full-stack web applications - will be accomplished by
writing in those languages only, leaving Sire mainly responsible for code that likely
needs to be jetted or otherwise optimized (like new crypto libraries, say).

Until that time, we'll be writing Sire. But since our immediate goal for the
moment is to build a cog that runs and does some stuff, we're not going to learn
every character and nuance of Sire syntax. Just enough.  
Don't worry if you don't fully understand _how_ a line of code does what it
does, but do make sure you have a handle on _what_ it's doing.

# Getting familiar

Open up a bootstrapped Sire REPL. As a reminder:

```
nix develop
plunder sire sire/boot.sire
```

We're going to hands-on learn a few basic concepts: Top-level bindings, let
bindings and function application.

A few meta-points to note at the outset:
- `;` is a comment in Sire. used here, it'll be to explain what's happening
inline
- The arrow keys won't work in the repl. Use backspace. If you get into a
strange state with carriage returns and are unable to backspace, just hit return
a few times, ignore any errors, and start again. Type carefully :)

## Top-level binding - `=`

Binds a value globally - not scoped.

In the repl:

```sire
x=3
; now just type an x and hit enter:

x ; the binding you are evaluating...
3 ; the value returned from the repl
```

## Function application - `|` or `()`

`add` is a function in Sire. It takes two arguments and adds them together. It's
called like this:

```sire
(add 1 3)
; ^ function name (add)
;    ^ first argument (1)
;      ^ second argument (3)

4   ; return value
```

You can also apply functions with `|`

```sire
| add 1 3
4   ; return value
```

Let's combine both of the above concepts to create our own named function. Note
the way in this case you start the line with the `=` (and a space) and the name of the
function is the first value after the opening parenthesis.


```sire
= (addTwo input)
| (add 2 input)
```

The repl now has a function named `addTwo` bound to its top-level scope. The
function takes a single value (called `input`) and all it does is call `add`
with two arguments, a hard-coded `2` and whatever input provided.

```sire
(addTwo 4)
6  ; return value
```

## Scoped/Let binding - `@`

`@` binds a value to the present scope.  
We can see this by trivially modifying our `addTwo` function to bind an
arbitrary variable and use that for the addition:

```sire
= (addTwo originalInput)
@ renamedInput originalInput
| add 2 renamedInput

(addTwo 4)
6
```

On the second line, we bound a new variable, `renamedInput` with the value of
`originalInput`. Then `2` is added to this variable.

To prove to yourself that `renamedInput` is only bound within the function
scope, try calling it at the top-level of the repl:

```sire
renamedInput

++ %crash
++ {Failed to Parse Sire}
++   `   # block renamedInput
         # where REPL:214
         # problem renamedInput
         # reason
           {++ {Failed to Parse Sire}
++   `   # block renamedInput
         # where REPL:214
         # problem renamedInput
         # reason {Unresolved symbol: renamedInput}
```


# TODO:

Don't read this part:

Syntax we'll need for the cog:

> Types
* inlined function application (?)
?? named and pinned lambda
b#{} - Bar, byte array. padded (?)
`: (x y z) < foo x y  | otherstufflater x`  - col macro (the thing on the left is the args to a function)
  - `foo x y` is the function call. the result of which is handled by the
  continuation (see below)
  - `(x y z)` is the continuation - what you want to do with result of `foo x
  y`.
# (as in # switch path) macro definition (wut? sire/switch.sire)
**SOME_CONST

Functions we'll need for the cog:

trk
fmapMaybe
hmLookup
barNat
natBar
barCat
getFiles (get* set* in general)
readRef
writeRef
newRef
syscall, fork
SOME (wut?)
map
weld
fromSome
tabLookup
tabFromPairs
barLen
hmInsert
hmSingleton
PIN

Data structures we'll need:

lists ~[], rows []
datacase, record

Other libraries we'll need to be familiar with:

json stuff (JMAP, JSTR, etc.)


main=() - cog running

The mechanic that's more interesting here w.r.t how the cog state works is `modifyState`.
Which is using `readRef` and `writeRef` to modify "slots" in the `kern.sire` state:

> Ref CogState > (CogState > CogState) > Cog ()
= (modifyState vSt fun return)
: (PIN old) < readRef vSt
@ srv       | **getServThread old
@ pNew      | PIN (fun old)
: _         < writeRef vSt pNew
: _         < cancelFork srv (syscall (**HTTP_SERV | fileServer pNew))
| return ()


