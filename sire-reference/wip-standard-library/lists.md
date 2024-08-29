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
listCatMap (\x -> CONS x (CONS x NIL)) (CONS 1 (CONS 2 NIL))  == CONS 1 (CONS 1 (CONS 2 (CONS 2 NIL)))
listCatMap (\x -> CONS (x + 1) NIL) (CONS 1 (CONS 2 (CONS 3 NIL)))  == CONS 2 (CONS 3 (CONS 4 NIL))
listCatMap (const NIL) (CONS 1 (CONS 2 NIL))  == NIL
```

### listTake

Returns the first n elements of a list.

```sire
listTake 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  == CONS 1 (CONS 2 NIL)
listTake 5 (CONS b#a (CONS b#b NIL))       == CONS b#a (CONS b#b NIL)
listTake 0 (CONS 1 (CONS 2 NIL))           == NIL
```

### listDrop

Drops the first n elements of a list and returns the rest.

```sire
listDrop 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  == CONS 3 NIL
listDrop 1 (CONS b#a (CONS b#b NIL))       == CONS b#b NIL
listDrop 5 (CONS 1 (CONS 2 NIL))           == NIL
```

### listTakeWhile

Takes elements from a list while a predicate holds.

```sire
listTakeWhile (< 3) (CONS 1 (CONS 2 (CONS 3 (CONS 4 NIL))))  == CONS 1 (CONS 2 NIL)
listTakeWhile isLower (CONS b#a (CONS b#b (CONS b#c (CONS b#d NIL))))  == CONS b#a (CONS b#b NIL)
listTakeWhile (const FALSE) (CONS 1 (CONS 2 NIL))  == NIL
```

### listDropWhile

Drops elements from the beginning of a list while a predicate function returns true.

This function takes a predicate function and a list as input. It removes elements from the front of the list as long as the predicate function returns true for each element. Once an element is encountered for which the predicate returns false, the function returns the remaining list.

```sire
listDropWhile (lth 3) ~[1 2 3 4 5]  == ~[3 4 5]
listDropWhile isEven ~[2 4 6 7 8 9] == ~[7 8 9]
listDropWhile (const FALSE) ~[1 2 3] == ~[1 2 3]
```

### listTakeWhile

Takes elements from the beginning of a list while a predicate function returns true.

```sire
listTakeWhile (lth 3) ~[1 2 3 4 5]  == ~[1 2]
listTakeWhile isEven ~[2 4 6 7 8 9] == ~[2 4 6]
listTakeWhile (const TRUE) ~[1 2 3] == ~[1 2 3]
```

### sizedListToRow

Converts a list to a row of a specified size, padding with zeros if necessary.

```sire
sizedListToRow 3 ~[1 2 3 4 5] == [1 2 3]
sizedListToRow 5 ~[1 2 3]     == [1 2 3 0 0]
sizedListToRow 0 ~[1 2 3]     == []
```

### sizedListToRowRev

Converts a list to a reversed row of a specified size, padding with zeros if necessary.

```sire
sizedListToRowRev 3 ~[1 2 3 4 5] == [3 2 1]
sizedListToRowRev 5 ~[1 2 3]     == [3 2 1 0 0]
sizedListToRowRev 0 ~[1 2 3]     == []
```

### listToRow

Converts a list to a row.

```sire
listToRow ~[1 2 3]     == [1 2 3]
listToRow ~[]          == []
listToRow ~[1 [2 3] 4] == [1 [2 3] 4]
```

### listToRowRev

Converts a list to a reversed row.

```sire
listToRowRev ~[1 2 3]     == [3 2 1]
listToRowRev ~[]          == []
listToRowRev ~[1 [2 3] 4] == [4 [2 3] 1]
```

### listRev

Reverses a list.

```sire
listRev ~[1 2 3]     == ~[3 2 1]
listRev ~[]          == ~[]
listRev ~[1 [2 3] 4] == ~[4 [2 3] 1]
```

### listSnoc

Appends an element to the end of a list.

```sire
listSnoc ~[1 2] 3      == ~[1 2 3]
listSnoc ~[] 1         == ~[1]
listSnoc ~[1 [2 3]] 4  == ~[1 [2 3] 4]
```

### listProd

Computes the Cartesian product of two lists.

```sire
listProd ~[1 2] ~[3 4]    == ~[[1 3] [1 4] [2 3] [2 4]]
listProd ~[1] ~[2 3 4]    == ~[[1 2] [1 3] [1 4]]
listProd ~[] ~[1 2]       == ~[]
```
