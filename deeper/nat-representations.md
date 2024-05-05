# A deeper look at how nats are represented in bars, strings and hexidecimal

```sire
showNat %a
{97}
```

We've [already](sire/intro.md) seen how the string `a` (entered above as `%a`) is "shown", stringified, as the ASCII value `97`, where 97 is a _decimal number_.

So what does the string `aa` look like? `97` twice? maybe the sum of `97` + `97`...?

```sire
showNat %aa
{24929}
```

Okay, that's surprising! To understand what's going on here, it would help us to realize that the _hexidecimal_ number 61, when converted to decimal, is `97`, which we remember is the ASCII character `a`.

We can prove this to ourselves by introducing a new representation. Similar to `b#` which represents an array of bytes in UTF-8 form, we can use `x#` to represent an array of bytes in hexidecimal form.

```
x#61
;; returns:
b#a

;; while
x#ba123
;; returns:
x#0ba123
```

What's going on here? How come sometimes we see a `b#`-formed printout and sometimes a `x#` one?

As we said above, the hexidecimal number 61 is `a` in ASCII (because it's 97 in decimal). So the REPL, in an effort to show us string-like things, is showing the byte array (of just the single byte that represents `a`) in UTF-8 form.  

But the hexidecimal number `ba123` (which, incidentally, is the decimal number `762147`) doesn't have a representation in ASCII, so the REPL just shows the byte array in hex form.

**Coming back to the `%aa` example we started at**: remember that `showNat` gave us `24929` for `%aa`. If you drop that number into a decimal -> hexidecimal converter, I promise it will give you `6161`. One way to look at that is the number "six thousand one hundred sixty-one". But another way is two `61`s side-by-side. `61` is the ASCII for `a`, so `6161` can function as a decimal (or integer or nat) representation for `aa`.  

There are two lessons to learn here: one is that at the end of the day, everything is stored as a natural number in memory, but these values can be represented different ways depending on the task at hand. The other is that the REPL can be misleading if you don't track _exactly_ what it's doing.
