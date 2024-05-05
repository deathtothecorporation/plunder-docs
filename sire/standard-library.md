---
description: 'Document Type: Reference'
---

# Abbreviated Standard Library Tour

Our goal at this point is to get you familiar with enough of the Sire "standard library" of functions that you feel comfortable writing a basic cog. As such, this is not an exhaustive reference but an overview of common functions.  

Feel free to skim this section for now and come back to it as needed while you work through building your first cogs.

For some of the sections, we note where more functions related to this area can be found in the `sire/` source files.

{% hint style="warning" %}
**All below are TODO, even the ones with some content:**
{% endhint %}

### `trk`

This is a printf, a `console.log`, a sigpam.

```Sire
(trk %something)

:: prints:
(_Trace %hello)
```

```Sire
= x %hello_world
| trk [%someMessage x]

;; prints:
trk=[%someMessage {hello_world}]
```

### `=?=`

Assertion. Crashes when the assertion is false. Often used for basic unit tests.

```sire
;; I say 1 is equal to 2!
=?= 1 2

;; Sire says it isn't:
++ %crash
++ {Failed to Parse Sire}
++   `   # block
           =?= 1 2
         # where REPL:35
         # problem
           =?= 1 2
         # reason
           {++ {Failed to Parse Sire}
++   `   # block
           =?= 1 2
         # where REPL:35
         # problem
           =?=
             * 1
             * 2
         # reason {ASSERTION FAILURE}
}


;; I say {hello} is equal to {hello}
=?= {hello} {hello}

;; The REPL agrees by not crashing and printing the assertion again
=?= {hello} {hello}
```

## Nats

`sire/sire_03_nat.sire`

### `showNat`

Mostly used in the REPL. Will print out a nat as a string.

```sire
showNat 100
{100}

showNat "a"
{97}

showNat "aa"
{24929}
```

### `natBar`

Given a nat, will return its bar representation.

```sire
natBar 97
b#a
```

## Bars

`sire/sire_16_bar.sire`

### `barNat`

Given a bar, returns it as a nat.

```sire
barNat b#a
%a
;; remember, this is the nat 97 but printed in stringified (ASCII) form

barNat x#1
1
barNat x#9
9
barNat x#a
10
;; remember, x# is representing a byte-array as hexidecimal, in which the
;; decimal number 10 is represented as hex 'a'.
```

### `barLen`

Given a bar, returns the length of the byte array.

```sire
barLen b#{}
0

barLen b#{a}
1

barLen b#{aaaaa}
5
```

### `barCat`

Given a row of bars, return their concatenation.

```sire
= b1 b#{hello}
= b2 b#{world}

| barCat [b1 b#{ } b2]

;; returns:
 b#{hello world}
```

### `barCatList`

Similar to `barCat`, but operates on a list, rather than a row (note the `~` in `~[]` below).

```sire
= b1 b#{hello}
= b2 b#{world}

| barCatList ~[b1 b#{ } b2]

;; returns:
 b#{hello world}
```

## Tabs

A tab is a map from noun to noun. Like a dict in Haskell or python.

`sire/sire_12_tab.sire`

### Create a tab: `#[]` / `_MkTab`

To create a tab, you can use the `_MkTab` function or the `#[]` syntax.  
`_MkTab` accepts two rows of nats for the keys and values, while `#[]` works more like a row of bindings.

```sire
= t | _MkTab [{one} {two} {three}] [1 2 3]
;; returns:
t=[one=1 two=2 three=3]

;; using the #[] syntax:
= t #[{one}=1 {two}=2 {three}=3]
;; returns:
t=[one=1 two=2 three=3]
```

In the above example, `one`, `two` and `three` are string keys; `1` `2` and `3` are nat values.  

Caution: While any nat can be used for keys and values, you might run into syntax issues when using the `#[]` structure:

```sire
;; this is a valid tab:
= t | _MkTab [b#{I'm a bar} {I'm a string}] [b#{bar value} {string value}]

;; but the same keys/values will choke the #[] version:
= t | #[b#{I'm a bar}={I'm a string}] [b#{bar value}={string value}]
++ %crash
++ {Failed to Parse Sire}
```

### `tabFromPairs`

Another way to construct a tab from a row of pairs:

```sire
= t | tabFromPairs [[{one} 1] [{two} 2] [{three} 3]]
[one=1 two=2 three=3]
```

### `tabGet`

Get the value at a key in a tab, return `0` if the key doesn't exist.

```sire
tabGet t {two}
2

tabGet t {i dont exist}
0
```

### `tabLookup`

(needs maybe)

- map
- weld

- fmapMaybe
- hmLookup
- hmInsert
- hmSingleton
- getFiles
  - `get*`, `set*` in general
- readRef
- writeRef
- newRef
- syscall, fork
- `SOME`
  - Probably move this to `Maybe` section in intro.md
- fromSome
- PIN

Next we'll introduce some more advanced topics that you'll come across:
