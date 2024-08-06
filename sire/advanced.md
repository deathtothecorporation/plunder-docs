---
description: 'Document Type: Reference'
---

# Recognize and move on

The topics here are explained so that you can recognize them if you come across them, but don't worry about fully understanding and using everything you see here. For the purposes of getting familiar with the system and building some toy cogs, you can get away with copying existing instances of these patterns and changing some details to suit your needs.  
Eventually you should understand everything you're doing, but let's keep you moving forward first.

## Types

Sire _sort of_ has types at the moment. This system will significantly change for the better in the future, but as a beginner we suggest you just ignore types for now. That said, you should at least be familiar with the syntax.

```sire
> Bar > Bar > Nat > Bit
= (barIsPrefixOf needle haystack off)
| eql needle
| barSlice off (barLen needle) haystack
```

This function takes a sought term "needle" and a target "haystack" and returns a boolean of whether or not this "needle" is the prefix of the "haystack" (at some offset).

This line is the type signature:

```sire
> Bar > Bar > Nat > Bit
```

First parameter is a bar (`needle`), second parameter is a bar (`haystack`), third parameter is a nat (`off`, for offset) and the function returns a bit (boolean).

## Lambdas

- `&`  Anonymous lambda
- `?`  Named lambda
- `??` Named and pinned lambda (Pinning has to do with memory layout. Don't worry about it too much for now, but you'll come across it in source files)

For example, here's `&` used infix:

```sire
| map x&(add x | mul x 2) [0 1 2]
[0 3 6]
```

And here it's used prefix:

```sire
| foreach [0 1 2]
& x
| add x | mul x 2
[0 3 6]
```

Both `map` and `foreach` do the same thing, they just accept their arguments in different orders. `map` takes a function and then a row, while `foreach` takes a row and then a function.

## Col macro

Using `&` prefix in the way we just did is quite uncommon in practice. We have a macro called `:` (pronounced "col") which does the same thing in one line less:

```sire
: x < foreach [0 1 2]
| add x | mul x 2
[0 3 6]
```

This macro-expands into exactly the same code as the version with `&`. The only difference is that it feels more like an assignment.

It's worth thinking about the shape of this a little bit more. If we had types, the type of `foreach` would be `Row a -> (a -> b) -> Row b`.
So `foreach` takes two arguments, a row and a function. We can clearly see this in the version that uses `&`: the first argument is the row `[0 1 2]` and the second argument spans lines 2 and 3.
But when we write it using the col macro, it *feels* as if we're only giving `foreach` a single argument, the row. And then somehow it *feels* as if `foreach` *returns* an `x` which we can use in the "loop body". In reality there is no loop body, and we're still passing a function as a second argument. But in many situations, this assignment-like syntax ends up communicating the intent much more clearly.

### Syscall continuations

For architectural reasons, it's quite common for us to want a value that represents "the rest of the program". For example, after a cog wakes up from a syscall, it needs to know what to do with the result. The natural way to express this is by using a continuation function (very similar to callback functions, which you might be familiar with).

For example, a cog that only gets the current time and then terminates would look like this:

```sire
:| prelude

= (timeCog return)
| syscall TIME_WHEN
& now
| return now
```

We're calling the `syscall` function with two arguments, the constant `TIME_WHEN` and a lambda in which we tell our future selves how we should handle the result of the `TIME_WHEN` syscall once it finishes. In this situation it's much clearer to use the col macro, because it highlights that `now` is actually the result of the syscall:

```sire
:| prelude

= (timeCog return)
: now < syscall TIME_WHEN
| return now
```

If you're coming from Haskell, you might recognize this as similar to `do`-notation. But note that we don't have anything like the `Monad` typeclass, so you have to supply the bind function explicitly, like we're doing with `syscall` here.

And once you start chaining multiple syscalls after each other, it's nice to not have to use two lines for every single one:

```sire
:| prelude

;; Just keep spinning grabbing the time and waiting forever.
= (infiniteTimeCog return)
: now < syscall TIME_WHEN
: _   < syscall (TIME_WAIT | inc now)
| infiniteTimeCog return
```

Compare this to using `&`:

```sire
:| prelude

;; Just keep spinning grabbing the time and waiting forever.
= (infiniteTimeCog return)
| syscall TIME_WHEN
& now
| syscall (TIME_WAIT | inc now)
& _
| infiniteTimeCog return
```

## `PIN`

Another one to note and ignore. All you need to know now is that Pins help with memory-layout optimization in the runtime. If you see `PIN` in a cog example somewhere, recognize it as a performance feature and move on for now.

---

Next up, running a simple Cog:
