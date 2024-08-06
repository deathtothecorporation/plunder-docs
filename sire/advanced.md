---
description: 'Document Type: Reference'
---

# Recognize and move on

The topics here are explained so that you can recognize them if you come across them, but don't worry about fully understanding and using everything you see here. For the purposes of getting familiar with the system and building some toy cogs, you can get away with copying existing instances of these patterns and changing them subtly to your needs.  
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

First parameter is a bar ("needle"), second parameter is a bar ("haystack"), third parameter is a nat (offset) and the function returns a bit (boolean).

## Lambdas

- `&`  Anonymous lambda
- `?`  Named lambda
- `??` Named and pinned lambda (Pinning has to do with helping with memory layout in persistence. Don't worry about it too much for now, but you'll come across it in source files)

## Col macro

```
; the 'syscall' function takes two arguments:
; - a request type of TIME_WHEN ("give me the current system time" in this case)
; - a continuation function
; we want to pass an argument to **the continuation** also, so
; we use & to define an anonymous lambda with one argument, "now" which
; is the result of the TIME_WHEN current time system call.
;
| syscall TIME_WHEN
& now
;
; We are in the body of the continuation function here and have the "now"
; binding included in scope. Once again we're seeing the continuation-passing
; style here.
```

"Col" as in "colon"

```sire
: a < foo x
; the rest of the computation below.
; a is in scope here
```
`foo x` is the function call. We can imagine its type signature would be `foo : a -> (b -> c) -> d`, in which case we would notice that it accepts a function as its second argument. When the final argument to a function is a continuation function, using the col macro is a way to do continuation passing that _feels_ like assignment.

`a` will be bound to the result of `foo x` and will be passed as the argument to `foo`'s final parameter, which is the rest of the code below it.

We'll use `gen` for examples (`gen` was covered in the [standard library](/sire/standard-library.md))

We previously saw gen used like this:
```sire
gen 10 | mul 2
[0 2 4 6 8 10 12 14 16 18]
```

Here it is with col:

```sire
: index < gen 5
| mul index 2
[0 2 4 6 8]
```

The same goal achieved with an anonymous lambda:

```sire
| gen 5
& i
| mul i 2

[0 2 4 6 8]
```

Here's an example with `maybeCase`, which we also saw earlier. Remember, `maybeCase` takes three arguments: a maybe, a default guard value to return when the value is NONE, and a function to call on the value when it is SOME. In this example, that final function is the continuation we want to handle legibly with the col macro.

```sire
maybeNine=(SOME 9)
: isItNine < maybeCase maybeNine {wasn't nine}
| inc isItNine

; returns:
10

maybeNine=NONE
: isItNine < maybeCase maybeNine {wasn't nine}
| inc isItNine

; returns:
{wasn't nine}
```

The same written with an anonymous lambda:

```sire
maybeNine=(SOME 9)
| maybeCase maybeNine {wasn't nine}
& isItNine
| inc isItNine

10


maybeNine=NONE
| maybeCase maybeNine {wasn't nine}
& isItNine
| inc isItNine

{wasn't nine}
```


This might look trivial (after all, you could have written `maybeCase (SOME 9) wasn't nine} inc` and arrived at `10` without using the col macro). But what if you don't have a simple function like `inc` to call? To drive the point home a bit more: Notice that in the next example, the remainder of the continued computation is acting on `actuallyNine` without having to use a let binding or order the function application pipeline strangely.

```sire
maybeNine=(SOME 9)
: actuallyNine < maybeCase maybeNine {wasn't nine}
| add 1 | div actuallyNine 3

; result:
4
```

One more time, with an anonymous lambda:

```sire
| maybeCase maybeNine {wasn't nine}
& actuallyNine
| add 1
| div actuallyNine 3

4
```

## `PIN`

Another one to note and ignore. All you need to know now is that Pins help with memory-layout optimization in the runtime. If you see `PIN` in a cog example somewhere, recognize it as a performance feature and move on for now.

---

Next up, running a simple Cog:
