# Lists

Lists are zero-terminated, nested row 2-tuples. They are declared by prepending a `~` to what looks like row syntax, like this: `~[]` (in the REPL we have to wrap this in parentheses):

### NIL

Evaluates to 0.

```sire
NIL    == 0    ; the empty list
```

### CONS

Constructs a new list by adding an element to the front of an existing list.

```sire
CONS 1 NIL                    == [1 0]            ; a list with one element
CONS b#a (CONS b#b NIL)       == [b#a [b#a 0]]    ; a list with two elements
CONS 1 (CONS 2 (CONS 3 NIL))  == [1 [2 [3 0]]]    ; a list with three elements
```

### listCase

Pattern matches on a list, providing cases for empty and non-empty lists.

{% hint style="danger" %}
```
TODO: find a nice example for listCase that doesn't require a lambda.
```
{% endhint %}

### listSing

Creates a singleton list containing one element.

```sire
listSing 5          == [5 0]
listSing b#hello    == [b#hello 0]
listSing []         == [[] 0]
```

### listMap

Applies a function to every element of a list.

```sire
listMap (mul 2) ~[1 2 3]                 == [2 [4 [6 0]]]
listMap isNat (CONS 3 (CONS b#a NIL))    == [1 [0 0]]
listMap id NIL                           == 0 ; NIL
```

### listForEach

Alias for `listMap`. Applies a function to every element of a list.

```sire
listForEach (CONS 1 (CONS 2 (CONS 3 NIL))) (mul 2)    == [2 [4 [6 0]]]
listForEach (~[3 [b#a 0]]) isNat                      == [1 [0 0]]
listForEach NIL id                                    == NIL
```

### listHead

Returns the first element of a list.

```sire
listHead (CONS 2 (CONS 3 NIL))    == (0 2)
listHead (CONS b#a NIL)           == (0 b#a)
listHead NIL                      == 0
```

### listSafeHead

Returns the first element of a list, or a fallback value if the list is empty.

```sire
listSafeHead 0 (CONS 1 (CONS 2 NIL))    == 1
listSafeHead b#x (CONS b#a NIL)         == b#a
listSafeHead b#default NIL              == b#default
```

### listUnsafeHead

Returns the first element of a list. Unsafe if the list is empty.

```sire
listUnsafeHead (CONS 1 (CONS 2 NIL))    == 1
listUnsafeHead (CONS b#a NIL)           == b#a
listUnsafeHead NIL                      == 0 ; NIL
```

### listUnsafeTail

Returns the tail of a list (all elements except the first). Unsafe if the list is empty.

```sire
listUnsafeTail (CONS 1 (CONS 2 NIL))    == [2 0]
listUnsafeTail (CONS b#a NIL)           == 0
listUnsafeTail NIL                      == 0 ; NIL
```

### listIdxCps

Continuation-passing style function to get the element at a given index in a list.

```sire
listIdxCps 1 (CONS b#a (CONS b#b NIL)) b#{not found} id    == b#b
listIdxCps 0 (CONS 1 NIL) b#{not found} id                 == 1
listIdxCps 2 (CONS 1 (CONS 2 NIL)) b#{not found} id        == b#{not found}
```

### listIdxOr

Returns the element at a given index in a list, or a fallback value if the index is out of bounds.

```sire
listIdxOr 0 1 (CONS b#a (CONS b#b NIL))       == b#b
listIdxOr b#z 99 (CONS b#a (CONS b#b NIL))    == b#z
listIdxOr b#default 0 NIL                     == b#default
```

### listIdx

Returns the element at a given index in a list, or 0 if the index is out of bounds.

```sire
listIdx 1 (CONS b#a (CONS b#b NIL))    == b#b
listIdx 0 (CONS 1 NIL)                 == 1
listIdx 2 (CONS 1 (CONS 2 NIL))        == 0
```

### listLastOr

Returns the last element of a list, or a fallback value if the list is empty.

```sire
listLastOr 0 (CONS 1 (CONS 2 NIL))    == 2
listLastOr b#z ~[b#a 0]               == 0
listLastOr b#z ~[]                    == b#z
```

### listUnsafeLast

Returns the last element of a list. Unsafe if the list is empty.

```sire
listUnsafeLast (CONS 1 (CONS 2 NIL))    == 2
listUnsafeLast (CONS b#a NIL)           == b#a
listUnsafeLast NIL                      == 0 ; NIL
```

### listLast

Returns the last element of a list.

```sire
listLast (CONS 1 (CONS 2 NIL))    == (0 2)
listLast ~[b#a]                   == (0 b#a)
listLast NIL                      == 0
```

### listFoldl

Left-associative fold of a list.

```sire
listFoldl add 0 ~[1 2 3]                 == 6
listFoldl barWeld b#{} ~[b#a b#b b#c]    == b#abc
listFoldl (flip CONS) NIL ~[1 2 3]       == [3 [2 [1 0]]]                    
```

### listFoldl1

Left-associative fold of a non-empty list, using the first element as the initial accumulator.

```sire
listFoldl1 add ~[2 3 4]                          == 9
listFoldl1 max (CONS 1 (CONS 5 (CONS 3 NIL)))    == 5
listFoldl1 barWeld ~[b#a b#b b#c]                == b#abc
```

### listFoldr

Right-associative fold of a list.

```sire
listFoldr sub 0 (~[1 2 3])               == 1
listFoldr barWeld b#{} ~[b#a b#b b#c]    == b#abc
listFoldr (flip CONS) NIL ~[1 2 3]       == [[[0 3] 2] 1]
```

### listLen

Computes the length of a list.

```sire
listLen (CONS 1 (CONS 2 (CONS 3 NIL)))    == 3
listLen (CONS b#a NIL)                    == 1
listLen NIL                               == 0
```

### listToRow

Converts a list to a row.

```sire
listToRow ~[1 2 3]  == [1 2 3]
listToRow (CONS b#a (CONS b#b NIL))       == [b#a b#b]
listToRow NIL                             == []
```

### sizedListToRow

Converts a list to a row of a specified size, padding with zeros if necessary.

```sire
sizedListToRow 3 ~[1 2]                            == [1 2 0]
sizedListToRow 2 (CONS 1 (CONS 2 (CONS 3 NIL)))    == [1 2]
sizedListToRow 4 NIL                               == [0 0 0 0]
```

### sizedListToRowRev

Converts a list to a row of a specified size in reverse order, padding with zeros if necessary.

```sire
sizedListToRowRev 3 (CONS 1 (CONS 2 NIL))    == [0 2 1]
sizedListToRowRev 2 ~[1 2 3]                 == [2 1]
sizedListToRowRev 4 NIL                      == [0 0 0 0]
```

### listToRowRev

Converts a list to a row in reverse order.

```sire
listToRowRev (CONS 1 (CONS 2 (CONS 3 NIL)))    == [3 2 1]
listToRowRev (~[b#a b#b])                      == [b#b b#a]
listToRowRev NIL                               == []
```

### listFromRow

Converts a row to a list.

```sire
listFromRow [1 2 3]       == [1 [2 [3 0]]]
listFromRow [b#a b#b]     == [b#a [b#b 0]]
listFromRow (gen 4 id)    == [0 [1 [2 [3 0]]]]
```

### listAnd

Returns TRUE if all elements in the list are TRUE, otherwise FALSE.

```sire
listAnd (CONS TRUE (CONS TRUE NIL))     == 1 ; TRUE
listAnd (CONS TRUE (CONS FALSE NIL))    == 0 ; FALSE
listAnd NIL                             == 1 ; TRUE
```

### listOr

Returns TRUE if any element in the list is TRUE, otherwise FALSE.

```sire
listOr (CONS FALSE (CONS TRUE NIL))    == 1 ; TRUE
listOr ~[FALSE 0]                      == 0 ; FALSE
listOr NIL                             == 0 ; FALSE
```

### listSum

Computes the sum of all elements in a list of numbers.

```sire
listSum (CONS 1 (CONS 2 (CONS 3 NIL)))    == 6
listSum (~[1 2 3])                        == 6
listSum NIL                               == 0
```

### listAll

Returns TRUE if all elements in the list satisfy the given predicate.

```sire
listAll even (CONS 2 (CONS 4 (CONS 6 NIL)))    == 1 ; TRUE
listAll (gte 1) (~[1 2 3])                     == 0 ; FALSE
listAll id NIL                                 == 1 ; TRUE
```

### listAllEql

Returns TRUE if all elements in the list are equal.

```sire
listAllEql (CONS 1 (CONS 1 (CONS 1 NIL)))    == 1 ; TRUE
listAllEql (~[b#a b#a])                      == 1 ; TRUE
listAllEql (CONS 1 (CONS 2 NIL))             == 0 ; FALSE
```

### listAny

Returns TRUE if any element in the list satisfies the given predicate.

```sire
listAny odd (CONS 2 (CONS 3 (CONS 4 NIL)))    == 1 ; TRUE
listAny (gte 0) (~[1 2 3])                    == 0 ; FALSE
listAny id NIL                                == 0 ; FALSE
```

### listHas

Checks if a list contains a specific element.

```sire
listHas 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  == 1 ; TRUE
listHas b#a (CONS b#b (CONS b#c NIL))     == 0 ; FALSE
listHas 1 NIL                             == 0 ; FALSE
```

### listWeld

Concatenates two lists.

```sire
listWeld (CONS 1 (CONS 2 NIL)) (CONS 3 (CONS 4 NIL))    == [1 [2 [3 [4 0]]]]
listWeld ~[b#a 0] ~[b#b 0]                              == [b#a [0 [b#b [0 0]]]]
listWeld NIL (CONS 1 NIL)                               == [1 0]
```

### listCat

Concatenates a list of lists into a single list.

```sire
listCat ~[~[1 [2 0]] ~[3 0]]            == [1 [[2 0] [3 [0 0]]]]
listCat ~[~[1 [2 0]] ~[b#b [b#c 0]]]    == [1 [[2 0] [b#b [[b#c 0] 0]]]]
listCat (CONS NIL (CONS NIL NIL))       == 0 ; NIL
```

### listCatMap

Applies a function to all elements in a list and concatenates the results.

```sire
listCatMap (x & ~[x x]) ~[1 2]      == [1 [1 [2 [2 0]]]]
listCatMap (x & ~[x]) ~[b#a b#b]    == [b#a [b#b 0]]
listCatMap (x & ~[]) ~[1 2 3]       == 0 ; NIL
```

### listTake

Takes the first n elements from a list.

```sire
listTake 2 (CONS 1 (CONS 2 (CONS 3 NIL)))    == [1 [2 0]]
listTake 3 ~[1 2]                            == [1 [2 0]]
listTake 0 (CONS 1 NIL)                      == 0 ; NIL
```

### listDrop

Drops the first n elements from a list.

```sire
listDrop 2 (CONS 1 (CONS 2 (CONS 3 NIL)))    == [3 0]
listDrop 3 ~[1 2]                            == 0 ; NIL
listDrop 0 (CONS 1 NIL)                      == [1 0]
```

### listTakeWhile

Takes elements from the front of a list while they satisfy a predicate.

```sire
listTakeWhile (gte 2) (CONS 1 (CONS 2 (CONS 3 (CONS 4 NIL))))    == [1 [2 0]]
listTakeWhile even ~[2 4 5 6]                                    == [2 [4 0]]
listTakeWhile (const TRUE) NIL                                   == 0 ; NIL
```

### listDropWhile

Drops elements from the front of a list while they satisfy a predicate.

```sire
listDropWhile (gte 2) (CONS 1 (CONS 2 (CONS 3 (CONS 4 NIL))))    == [3 [4 0]]
listDropWhile even ~[2 4 5 6]                                    == [5 [6 0]]
listDropWhile (const TRUE) NIL                                   == 0 ; NIL
```

### listZipWith

Combines two lists element-wise using a given function.

```sire
listZipWith add (CONS 1 (CONS 2 NIL)) (CONS 3 (CONS 4 NIL))    == [4 [6 0]]
listZipWith v2 ~[1 2 3] ~[4 5]                                 == [[1 4] [[2 5] 0]]
listZipWith mul NIL (CONS 1 NIL)                               == 0 ; NIL
```

### listZip

Combines two lists into a list of pairs.

```sire
listZip (CONS 1 (CONS 2 NIL)) (CONS 3 (CONS 4 NIL))    == [[1 3] [[2 4] 0]]
listZip ~[1 2 3] ~[4 5]                                == [[1 4] [[2 5] 0]]
listZip NIL (CONS 1 NIL)                               == 0 ; NIL
```

### listFilter

Keeps only the elements of a list that satisfy a predicate.

```sire
listFilter even (CONS 1 (CONS 2 (CONS 3 (CONS 4 NIL))))    == [2 [4 0]]
listFilter (lte 3) ~[1 2 3 4 5]                            == [3 [4 [5 0]]]
listFilter (const TRUE) NIL                                == 0 ; NIL
```

### listIsEmpty

Checks if a list is empty.

```sire
listIsEmpty NIL                      == 1 ; TRUE
listIsEmpty (CONS 1 NIL)             == 0 ; FALSE
listIsEmpty (CONS 1 (CONS 2 NIL))    == 0 ; FALSE
```

### listSortOn

Sorts a list based on a key function.

```sire
listSortOn id (CONS 3 (CONS 1 (CONS 2 NIL)))    == [1 [2 [3 0]]]
listSortOn (div 1) ~[1 2 3]                     == [3 [2 [1 0]]]
listSortOn len ~[[1 2] [3] [4 5 6]]             == [[3] [[1 2] [[4 5 6] 0]]]
```

### listNub

Removes duplicate elements from a list, keeping only the first occurrence.

```sire
listNub (CONS 1 (CONS 2 (CONS 1 (CONS 3 NIL))))    == [1 [2 [3 0]]]
listNub ~[3 2 1 2 1]                               == [3 [2 [1 0]]]
listNub NIL                                        == 0 ; NIL
```

### listIterate

Generates an infinite list by repeatedly applying a function to an initial value.

```sire
listTake 5 (listIterate inc 0)        == [0 [1 [2 [3 [4 0]]]]]
listTake 3 (listIterate (mul 2) 1)    == [1 [2 [4 0]]]
listTake 0 (listIterate id 1)         == 0 ; NIL
```

### listGen

Generates a list of n elements using a given function.

```sire
listGen 3 id             == [0 [1 [2 0]]]
listGen 4 (const b#a)    == [b#a [b#a [b#a [b#a 0]]]]
listGen 0 (const 1)      == 0 ; NIL
```

### listRep

Generates a list of n copies of a given element.

```sire
listRep 1 3      == [1 [1 [1 0]]]
listRep b#a 2    == [b#a [b#a 0]]
listRep 0 0      == 0 ; NIL
```

### listElemIndex

Finds the index of the first occurrence of an element in a list.

```sire
listElemIndex 2 (CONS 1 (CONS 2 (CONS 3 NIL))) NONE SOME    == (0 1)
listElemIndex b#a ~[b#b b#c b#c b#a] NONE SOME              == (0 3)
listElemIndex 4 ~[1 2 3] NONE SOME                          == 0
```

### listIsPrefixOf

Checks if one list is a prefix of another list.

```sire
listIsPrefixOf (CONS 1 (CONS 2 NIL)) (CONS 1 (CONS 2 (CONS 3 NIL)))    == 1 ; TRUE
listIsPrefixOf ~[1 2] ~[1 2 3]                                         == 1 ; TRUE
listIsPrefixOf ~[1 2] ~[2 1]                                           == 0 ; FALSE
```

### listIndexed

Pairs each element in a list with its index.

```sire
listIndexed (CONS 1 (CONS 2 (CONS 3 NIL)))    == [[0 1] [[1 2] [[2 3] 0]]]
listIndexed ~[b#a b#b]                        == [[0 b#a] [[1 b#b] 0]]
listIndexed NIL                               == 0 ; NIL
```

### listIntersperse

Intersperses an element between every element of a list.

```sire
listIntersperse 0 (CONS 1 (CONS 2 (CONS 3 NIL)))    == [1 [0 [2 [0 [3 0]]]]]
listIntersperse b#a ~[b#b]                          == [b#b 0]
listIntersperse 0 NIL                               == 0 ; NIL
```

### listRev

Reverses a list.

```sire
listRev (CONS 1 (CONS 2 (CONS 3 NIL)))    == [3 [2 [1 0]]]
listRev ~[b#a b#b]                        == [b#b [b#a 0]]
listRev NIL                               == 0 ; NIL
```

### listSnoc

Adds an element to the end of a list.

```sire
listSnoc (CONS 1 (CONS 2 NIL)) 3    == [1 [2 [3 0]]]
listSnoc ~[b#a] b#b                 == [b#a [b#b 0]]
listSnoc NIL 1                      == [1 0]
```

### listProd

Computes the Cartesian product of two lists.

```sire
listProd (CONS 1 (CONS 2 NIL)) (CONS 3 (CONS 4 NIL))    == [[1 3] [[1 4] [[2 3] [[2 4] 0]]]]
listProd ~[1 2] ~[b#a b#b]                              == [[1 b#a] [[1 b#b] [[2 b#a] [[2 b#b] 0]]]]
listProd NIL (CONS 1 NIL)                               == 0 ; NIL
```
