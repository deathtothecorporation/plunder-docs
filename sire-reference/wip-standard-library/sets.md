# Sets

Data jetted sets of nouns. Sets are represented as a `law` where the row has no duplicate elements and all elements are stored in ascending order, with the form:

```
(0 1 2 row)
```

## Set Functions

### isSet

Checks if a value is a valid set.

```sire
isSet %[]           == 1
isSet %[1 2 3]      == 1
isSet (0 1 2 [])    == 1
isSet (0 2 2 [])    == 0  ; Invalid set representation
isSet [1 2 3]       == 0  ; Not a set, just a row
```

### emptySet

Returns an empty set.

```sire
emptySet    == %[]
emptySet    == setFromRow []
emptySet    == setDel 1 %[1]
```

### setIsEmpty

Checks if a set is empty.

```sire
setIsEmpty emptySet    == 1
setIsEmpty %[1]        == 0
setIsEmpty %[1 2 3]    == 0
```

### setSing

Creates a singleton set containing one element.

```sire
setSing 3      == %[3]
setSing {a}    == %[a]
setSing 0      == %[0]
```

### setFromRow

Creates a set from a row, removing duplicates and sorting.

```sire
setFromRow [3 1 2 1]        == %[1 2 3]
setFromRow [{a} {b} {a}]    == %[a b]
setFromRow [5 4 3 2 1]      == %[1 2 3 4 5]
```

### setFromRowAsc

Creates a set from a row that is already in ascending order with no duplicates.

```sire
setFromRowAsc [1 2 3]          == %[1 2 3]
setFromRowAsc [{a} {b} {c}]    == %[a b c]
setFromRowAsc [0]              == %[0]
```

### setToRow

Converts a set to a row.

```sire
setToRow %[1 2 3]    == [1 2 3]
setToRow %[a b c]    == [a b c]
setToRow %[]         == []
```

### setLen

Returns the number of elements in a set.

```sire
setLen %[]         == 0
setLen %[1 2 3]    == 3
setLen %[a]        == 1
```

### setToList

Converts a set to a list.

```sire
setToList %[1 2 3]    == [1 [2 [3 0]]]
setToList %[a b]      == [%a [%b 0]]
setToList %[]         == 0  ; NIL
```

### setFoldl

Left-associative fold over a set.

```sire
setFoldl add 0 %[1 2 3]              == 6
setFoldl mul 1 %[1 2 3 4]            == 24
setFoldl (flip CONS) NIL %[1 2 3]    == [3 [2 [1 0]]]
```

### setFoldr

Right-associative fold over a set.

```sire
setFoldr (flip CONS) NIL %[1 2 3]     == [[[0 3] 2] 1]
setFoldr sub 0 %[1 2 3]               == 1
setFoldr strWeld {} %[{a} {b} {c}]    == %abc
```

### setIns

Inserts an element into a set.

```sire
setIns 3 %[1 2]      == %[1 2 3]
setIns 2 %[1 2]      == %[1 2]  ; No change if element already exists
setIns {a} %[b c]    == %[a b c]
```

### setDel

Removes an element from a set.

```sire
setDel 2 %[1 2 3]      == %[1 3]
setDel 4 %[1 2 3]      == %[1 2 3]  ; No change if element doesn't exist
setDel {b} %[a b c]    == %[a c]
```

### setHas

Checks if an element is in a set.

```sire
setHas 2 %[1 2 3]      == 1
setHas 4 %[1 2 3]      == 0
setHas {b} %[a b c]    == 1
```

### setWeld

Combines two sets.

```sire
setWeld %[1 2] %[2 3]    == %[1 2 3]
setWeld %[a b] %[c d]    == %[a b c d]
setWeld %[] %[1 2 3]     == %[1 2 3]
```

### setUnion

Alias for setWeld. Combines two sets.

```sire
setUnion %[1 2] %[2 3]    == %[1 2 3]
setUnion %[a b] %[c d]    == %[a b c d]
setUnion %[] %[1 2 3]     == %[1 2 3]
```

### setCatRow

Combines a row of sets into a single set.

```sire
setCatRow [%[1 2] %[2 3] %[3 4]]    == %[1 2 3 4]
setCatRow [%[a b] %[c] %[d e]]      == %[a b c d e]
setCatRow [%[] %[1] %[]]            == %[1]
```

### setCatList

Combines a list of sets into a single set.

```sire
setCatList [%[1 2] [%[3 4] [%[2 3] 0]]]    == %[1 2 3 4]
setCatList [%[a b] [%[c] [%[d e] 0]]]      == %[a b c d e]
setCatList [%[] [%[1] [%[] 0]]]            == %[1]
```

### setCatRowAsc

Combines a row of sets that are already in ascending order.

```sire
setCatRowAsc [%[1 2] %[3 4] %[5 6]]    == %[1 2 3 4 5 6]
setCatRowAsc [%[a b] %[c d] %[e f]]    == %[a b c d e f]
setCatRowAsc [%[] %[1] %[2 3]]         == %[1 2 3]
```

### setMin

Returns the minimum element in a set.

```sire
setMin %[1 2 3]    == 1
setMin %[c b a]    == a
setMin %[5]        == 5
```

### setMax

Returns the maximum element in a set.

```sire
setMax %[1 2 3]    == 3
setMax %[c b a]    == c
setMax %[5]        == 5
```

### setPop

Removes and returns the minimum element from a set.

```sire
setPop %[1 2 3]    == [1 %[2 3]]
setPop %[a b c]    == [%a %[b c]]
setPop %[5]        == [5 %[]]
```

### setDrop

Removes the first n elements from a set.

```sire
setDrop 2 %[1 2 3 4]    == %[3 4]
setDrop 1 %[a b c]      == %[b c]
setDrop 0 %[1 2 3]      == %[1 2 3]
```

### setTake

Keeps the first n elements of a set.

```sire
setTake 2 %[1 2 3 4]    == %[1 2]
setTake 3 %[a b c d]    == %[a b c]
setTake 0 %[1 2 3]      == %[]
```

### setSplitAt

Splits a set at a given index.

```sire
setSplitAt 2 %[1 2 3 4]    == [%[1 2] %[3 4]]
setSplitAt 1 %[a b c]      == [%[a] %[b c]]
setSplitAt 0 %[1 2 3]      == [%[] %[1 2 3]]
```

### setSplitLT

Splits a set into elements less than a given value and the rest.

```sire
setSplitLT 3 %[1 2 3 4 5]      == [%[1 2] %[3 4 5]]
setSplitLT {c} %[a b c d e]    == [%[a b] %[c d e]]
setSplitLT 0 %[1 2 3]          == [%[] %[1 2 3]]
```

### setIntersect

Returns the intersection of two sets.

```sire
setIntersect %[1 2 3] %[2 3 4]    == %[2 3]
setIntersect %[a b c] %[b c d]    == %[b c]
setIntersect %[1 2 3] %[4 5 6]    == %[]
```

### setSub

Subtracts one set from another.

```sire
setSub %[1 2 3] %[2 3]      == %[1]
setSub %[a b c d] %[b d]    == %[a c]
setSub %[1 2 3] %[4 5 6]    == %[1 2 3]
```

### setElem

Returns the nth element of a set.

```sire
setElem 1 %[1 2 3]    == 2
setElem 0 %[a b c]    == %a
setElem 2 %[x y z]    == %z
```

### setDifference

Alias for setSub.

### setInsert

Alias for setIns.

### setSubtract

Alias for setSub.

### setIntersection

Alias for setIntersect.
