# Intro

## Intro to Sire

As already discussed, the _point_ of Sire is to get back PLAN. It is also possible to compile mainstream functional languages to PLAN. At that point, much Plunder development—including full-stack web applications—will be accomplished by writing in those languages only, leaving Sire only responsible for code that needs to be optimized by the runtime (like new crypto libraries, say).

Until that time, we'll be writing Sire. But since our immediate goal for the moment is to build a cog that runs and does some stuff, we're not going to learn every character and nuance of Sire syntax. Just enough.  
Don't worry if you don't fully understand _how_ a line of code does what it does, but do make sure you have a handle on _what_ it's doing.

## Getting familiar

Open up a bootstrapped Sire REPL. As a reminder:

```
nix develop
plunder sire sire/prelude.sire
```

Or if you're using the docker images:

```
docker pull deathtothecorporation/sire-repl
docker run -it deathtothecorporation/sire-repl
```

We're going to hands-on learn a few basic concepts: Top-level bindings, let bindings and function application.

A few meta-points to note at the outset:

* `;` is a comment in Sire. used here, it'll be to explain what's happening inline
* The arrow keys won't work in the REPL. Use backspace. If you get into a strange state with carriage returns and are unable to backspace, just hit return a few times, ignore any errors, and start again.
* The REPL will print ASCII values of integers. If you enter `65` you'll get back `%A`. Don't think about this too much yet. We'll cover this in detail on the next page.

### Top-level binding - `=`

Binds a value globally - not scoped.

In the REPL:

```sire
x=3
; now just type x and hit enter:

x ; the binding you are evaluating...
3 ; the value returned from the REPL
```

### Function application - `|`, `()`, `-`

`add` is a function in Sire. It takes two arguments and adds them together. It's called like this:

```sire
(add 1 3)
; ^ function name (add)
;    ^ first argument (1)
;      ^ second argument (3)

4 ; return value
```

You can also apply functions with `|`

```sire
| add 1 3
4   ; return value
```

A less common style for function application that you might see is `-`:

```
add-1-3
4
```

Let's combine both of the above concepts to create our own named function. Note the way in this case you start the line with `=` (followed by a space) and the name of the function is the first value after the opening parenthesis.

```sire
= (addTwo input)
| add 2 input
```

The REPL now has a function named `addTwo` bound to its top-level scope. The function takes a single value (called `input`) and all it does is apply (with `|`) the `add` function to two arguments, a hard-coded `2` and whatever input was provided.

```sire
(addTwo 4)
6  ; return value
```

### Scoped/Let binding - `@`

`@` binds a value to the present scope.\
We can see this by trivially modifying our `addTwo` function to bind an arbitrary name and use that for the addition:

```sire
= (addTwo originalInput)
@ renamedInput originalInput
| add 2 renamedInput

(addTwo 4)
6
```

On the second line, we bound a new name, `renamedInput` with the value of `originalInput`. Then `2` is added to it.

To prove to yourself that `renamedInput` is only bound within the function scope, try calling it at the top-level of the REPL:

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

## Natural numbers, byte-arrays and strings

The big takeaway from this section has to do with natural numbers, but we have to get a thing or two out of the way first about how the REPL deals with strings.

The REPL renders strings in single curly braces, `{like this example}`. You can also enter a string this way, as in `= msg {hello world}` in order to bind the name `msg` to the string `hello world`.

For a string without spaces, the REPL will opt to render it with a leading `%` as in `%houseboat`. Again, you can enter strings this way as well: `= color %blue` (which binds the string `blue` to the name `color`). You may sometimes see this style referred to as an "atom" in some source files.

It's worth mentioning that you _can enter_ a string with double-quotes as you may expect, but the REPL never prints them that way:

```
= msg "hello world"
msg
;; prints:
{hello world}
```
It is for this reason we suggest you get used to the `{}` and `%` style.

### Nat - natural number

All values in Plunder are stored in memory as natural numbers. Throughout the system, these are referred to as "nats". They're sometimes also called "nouns".  
The REPL will represent these values as ASCII for printing purposes, which can be confusing at first and should be noted up front. For instance, the character `a` is encoded as `97` in ASCII:

```sire
97
;; prints:
%a

| showNat %a
;; prints:
{97}
;; The showNat function represents a nat as a string.
;; Single curly braces wrapping a value means you're seeing a string.
;; Think of {curly braces} as you would "double quotes", as far as the
;; REPL is concerned.
```

If this nat/string stuff is driving you crazy, the full explanation is [here](/deeper/nat-representations.md).

### Bar - byte-array, and Strings

A "bar" is an array of UTF-8 bytes. You declare a bar like this:

```sire
b#{some stuff here}
```

You only need to use the curly braces when spaces are present in your byte array:

```
b#thisIsAFineBar
```

Although for many purposes you can think of bars and strings interchangeably. One difference is that there are more standard library functions for working with bars on a structural level (folding, splitting at indexes, filtering, etc) while the string standard library functions are more geared to character-level functions like capitalization and checking if a character is an alphanumeric.  

Because a bar is an array and the byte width of its constituent parts is important for it to be a proper representation, it sort of acts as a "string with some extra baggage around it".  
Converting between bars and strings is trivial for the cases where you need string functions but you have a bar at hand (or vice-versa). We'll see more on this on the next page. The conversions are basically about removing or adding that "extra baggage" mentioned above.

Trust us that you can and should basically just use bars for everything string-like and you can move along to the section on data structures below, but if you're interested in seeing the deep dive, it's [here](/deeper/nat-representations.md).

## A Few Simple Data Structures

### Rows - `[]`

Rows/vectors are basically arrays (not Lists - see below). They are defined with `[ ]`:

```sire
arr=[10 64 42]

arr
[10 64 42]
```

We'll get into more of the standard library/convenience functions later, but we'll need a few now, too, in order to prove to ourselves some details of rows. `idx` is used to get the value at a given index in a row:

```sire
arr=[10 64 42]

(idx 0 arr)
10
; the zeroth item in the row

(idx 2 arr)
42

(idx 50 arr)
0
; you'll get zero back if you overshoot
```

`len` will give you the size of the row:

```sire
(len arr)
3
```

### Lists - `~[]`

Lists are zero-terminated, nested row 2-tuples. They are declared by prepending a `~` to what looks like row syntax, like this: `~[]` (in the REPL we have to wrap this in parentheses):

```sire
x=(~[10 64 42])

[10 [64 [42 0]]]

(idx 0 x)
10

(idx 1 x)
[64 [42 0]]
```

Incidentally, you can create a list from a row with the `listFromRow` function:

```
y=[2 3 4]
listy=(listFromRow y)

listy
[2 [3 [4 0]]]
```

## Comparison, Booleans and Maybes

`TRUE` is represented as the nat `1`, while `FALSE` is the nat `0`.

```sire
=?= 1 | TRUE
=?= 0 | FALSE
```

### `eql` and friends

The `eql` function takes two parameters and returns `TRUE` if they're equal)

```sire
eql 1 1
1

eql 1 999
0
```

Similarly, we have `neq` for "not equal". Also:

- `lth` - true when first parameter is less than second
- `lte` - true when first parameter is less then or equal to second
- `gth` - true when first parameter is greater than second
- `gte` - true when first parameter is greater than or equal to second

### `if`

`if` is a function with three parameters: what is being tested for truthiness; what to return if true; what to return if false.

```sire
| if 1 {the if got a true} {the if got a false}
; returns:
{the if got a true}
```

The `if` function's first parameter was `1`, a true value, so the second parameter was returned.

### `not`

Negation.

```sire
| not FALSE
; returns:
1
```

### `and`, `or`

`and` and `or` are functions that do what you'd expect:

```sire
and 1 1
;; returns:
1

and 1 0
;; returns:
0

or 1 0
;; returns:
1

or 0 0
;; returns:
0
```

## Moving on

This was a brief overview of the nuts and bolts of Sire. Printing out bars in the REPL is fun and all, but our goal is to build a web app, not test the limit of how many "hello world" strings we can fit in our terminal scrollback.  

Next we'll take a look at a small sample of the standard library that we'll use while building our first Plunder Cog.
