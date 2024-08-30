# Rows

{% hint style="warning" %}
This page is under active development. It may contain bugs or incomplete descriptions.

TODO: Update with function and type signatures.
{% endhint %}

This module defines operations on rows, which are [data-jetted](../../deeper/jets.md) arrays. Since PLAN is untyped, these are also used as the building blocks for tuples, records, and datatypes.

They are defined with `[]`:

```sire
arr=[10 64 42]

arr
[10 64 42]
```

### null

Checks if a row is empty.

`null` returns `TRUE` if the given row is empty, and `FALSE` otherwise.

```sire
null []         == 1
null [1 2 3]    == 0
null [0]        == 0
```

### arity

Returns the arity of a function or row.

`arity` returns the number of arguments a function takes, or the number of elements in a row plus one.

```sire
arity add        == 2
arity [1 2 3]    == 4
arity []         == 1
```

### len

Returns the length of a row.

`len` counts the number of elements in a given row.

```sire
len [1 2 3]    == 3
len []         == 0
len [9]        == 1
```

### idx

Retrieves an element from a row at a specified index.

`idx` returns the element at the given index in the row. Indexing starts at 0.

```sire
idx 1 [10 20 30]    == 20
idx 0 [5 6 7]       == 5
idx 2 [1]           == 0 (out of bounds)
```

### get

Retrieves an element from a row at a specified index.

`get` is an alias for `idx`. It returns the element at the given index in the row.

```sire
get [10 20 30] 1    == 20
get [5 6 7] 0       == 5
get [1] 2           == 0 (out of bounds)
```

### mut

Modifies an element in a row at a specified index.

`mut` returns a new row with the element at the given index replaced by the provided value.

```sire
mut 1 99 [10 20 30]    == [10 99 30]
mut 0 5 [1 2 3]        == [5 2 3]
mut 3 4 [1 2]          == [1 2 0 4]
```

### put

Modifies an element in a row at a specified index.

`put` is an alias for `mut`. It returns a new row with the element at the given index replaced by the provided value.

```sire
put [10 20 30] 1 99    == [10 99 30]
put [1 2 3] 0 5        == [5 2 3]
put [1 2] 3 4          == [1 2 0 4]
```

### switch

Selects a value from a row based on an index, with a fallback value.

`switch` returns the element at the given index if it exists, otherwise it returns the fallback value.

```sire
switch 1 0 [10 20 30]    == 20
switch 3 0 [10 20 30]    == 0 (fallback)
switch 0 99 []           == 99 (fallback)
```

### c0, c1, c2, c3, c4, c5, c6, c7, c8, c9

Creates a row constructor of the specified arity.

These functions create row constructors of arity 0 to 9 respectively.

```sire
c3 1 2 3      == [3 2 1]
c0            == []
c2 b#a b#b    == [b#b b#a]
```

### v0, v1, v2, v3, v4, v5, v6, v7, v8, v9

Creates a row of the specified arity.

These functions create rows of arity 0 to 9 respectively, with the arguments in reverse order.

```sire
v3 1 2 3      == [1 2 3]
v0            == []
v2 b#a b#b    == [b#a b#b]
```

### isRow

Checks if a value is a row.

`isRow` returns `TRUE` if the given value is a row, and `FALSE` otherwise.

```sire
isRow [1 2 3]    == 1
isRow []         == 1
isRow 3          == 0
```

### weld

Concatenates two rows.

`weld` combines two rows into a single row, with the elements of the first row followed by the elements of the second row.

```sire
weld [1 2] [3 4]    == [1 2 3 4]
weld [] [5 6]       == [5 6]
weld [1 2] []       == [1 2]
```

### gen

Generates a row based on a function and a length.

`gen` creates a row of the specified length, where each element is the result of applying the given function to its index.

```sire
gen 3 (add 2)    == [2 3 4]
gen 4 id         == [0 1 2 3]
gen 0 | mul 2    == []
```

### fst

Returns the first element of a row.

`fst` retrieves the leftmost element of a given row.

```sire
fst [1 2 3]    == 1
fst [9]        == 9
fst []         == 0
```

### snd

Returns the second element of a row.

`snd` retrieves the second element of a given row. If the row has fewer than two elements, it returns 0.

```sire
snd [1 2 3]    == 2
snd [9]        == 0
snd []         == 0
```

### thr

Returns the third element of a row.

`thr` retrieves the third element of a given row. If the row has fewer than three elements, it returns 0.

```sire
thr [1 2 3 4]    == 3
thr [1 2]        == 0
thr []           == 0
```

### map

Applies a function to each element of a row.

`map` creates a new row by applying the given function to each element of the input row.

```sire
map (mul 2) [1 2 3]            == [2 4 6]
map fst [[1 2] [3 4] [5 6]]    == [1 3 5]
map (add 1) []                 == []
```

### foreach

Alias for `map` with arguments reversed.

`foreach` is an alias for `map`. It applies a function to each element of a row.

```sire
foreach [1 2 3] (mul 2)            == [2 4 6]
foreach [[1 2] [3 4] [5 6]] fst    == [1 3 5]
foreach [] (add 1)                 == []
```

### rev

Reverses a row.

`rev` creates a new row with the elements of the input row in reverse order.

```sire
rev [1 2 3]    == [3 2 1]
rev [9]        == [9]
rev []         == []
```

### rowCons

Prepends an element to a row.

`rowCons` creates a new row with the given element as the first element, followed by all elements of the input row.

```sire
rowCons 1 [2 3]    == [1 2 3]
rowCons b#a []     == [b#a]
rowCons 0 [1]      == [0 1]
```

### rowSnoc

Appends an element to a row.

`rowSnoc` creates a new row with all elements of the input row, followed by the given element as the last element.

```sire
rowSnoc [1 2] 3    == [1 2 3]
rowSnoc [] b#a     == [b#a]
rowSnoc [0] 1      == [0 1]
```

### rowApply

Applies a function to a row of arguments.

`rowApply` takes a function and a row of arguments, and applies the function to those arguments.

```sire
rowApply add [2 3]    == 5
rowApply gte [3 4]    == 0
rowApply lte [3 4]    == 1
```

### rowRepel

Applies a function to a row of arguments in reverse order.

`rowRepel` takes a function and a row of arguments, and applies the function to those arguments in reverse order.

```sire
rowRepel add [2 3]    == 5
rowRepel gte [3 4]    == 1
rowRepel lte [3 4]    == 0
```

### slash

Extracts a slice from a row, from index `s` to index `e`, padding with zeros if necessary.

```sire
slash [1 2 3 4 5] 1 3    == [2 3]
slash [1 2 3] 0 5        == [1 2 3 0 0]
slash [1 2 3] 2 2        == []
```

### slice

Similar to `slash`, but doesn't pad with zeros. It returns a slice from index `s` up to (but not including) index `e`.

```sire
slice [1 2 3 4 5] 1 3    == [2 3]
slice [1 2 3] 0 5        == [1 2 3]
slice [1 2 3] 2 2        == []
```

### chunks

Splits a row into chunks of a specified size.

```sire
chunks 2 [1 2 3 4 5]    == [[1 2] [3 4] [5]]
chunks 3 [1 2 3 4]      == [[1 2 3] [4]]
chunks 5 [1 2 3]        == [[1 2 3]]
```

### rep

Creates a row by repeating a value a specified number of times.

```sire
rep 3 2         == [3 3]
rep b#aaab 3    == [b#aaab b#aaab b#aaab]
rep [] 2        == [[] []]
```

### rowIndexed

Creates a row of pairs, where each pair contains the index and the corresponding element from the input row.

```sire
rowIndexed [10 20 30]       == [[0 10] [1 20] [2 30]]
rowIndexed [b#aba b#bab]    == [[0 b#aba] [1 b#bab]]
rowIndexed []               == []
```

### findIdx

Finds the index of the first element in a row that satisfies a predicate function.

```sire
findIdx (lte 5) [1 3 5 7 9] 0 id           == 2
findIdx (lte 10) [1 3 5 7 9] 0 id          == 0
findIdx even [1 3 5 7] b#{not found} id    == b#{not found}
```

### elemIdx

Finds the index of the first occurrence of a specific element in a row.

```sire
elemIdx 5 [1 3 5 7 5] b#{not found} id        == 2
elemIdx b#a [b#b b#a b#c] b#{not found} id    == 1
elemIdx 4 [1 2 3] b#{not found} id            == b#{not found}
```

### has

Checks if a row contains a specific element.

```sire
has 3 [1 2 3 4 5]      == 1
has b#a [b#b b#c]      == 0
has [] [[1] [2] []]    == 1
```

### rowAnd

Performs a logical AND operation on all elements of a row.

```sire
rowAnd [TRUE TRUE FALSE]    == 0
rowAnd [TRUE TRUE TRUE]     == 1
rowAnd []                   == 1
```

### rowOr

Performs a logical OR operation on all elements of a row.

```sire
rowOr [FALSE FALSE TRUE]     == 1
rowOr [FALSE FALSE FALSE]    == 0
rowOr []                     == 0
```

### sum

Calculates the sum of all elements in a row.

```sire
sum [1 2 3 4 5]          == 15
sum [10 (sub 10 3) 3]    == 20
sum []                   == 0
```

### sumOf

Applies a function to each element of a row and then calculates the sum of the results.

```sire
sumOf (mul 2) [1 2 3 4]    == 20
sumOf (pow 2) [1 2 3]      == 14
sumOf id []                == 0
```

### all

Checks if all elements in a row satisfy a given predicate.

```sire
all even [2 4 6 8]     == 1
all (lte 0) [1 2 3]    == 1
all id []              == 1
```

### any

Checks if any element in a row satisfies a given predicate.

```sire
any odd [2 4 5 8]        == 1
any (gte 0) [1 2 3 4]    == 0
any id []                == 0
```

### zip

Combines two rows into a row of pairs.

```sire
zip [1 2 3] [b#a b#b b#c]    == [[1 b#a] [2 b#b] [3 b#c]]
zip [1 2] [b#a b#b b#c]      == [[1 b#a] [2 b#b]]
zip [] [1 2 3]               == []
```

### zipWith

Combines two rows using a given function.

```sire
zipWith add [1 2 3] [4 5 6]                == [5 7 9]
zipWith mul [1 2 3] [1 2 3]                == [1 4 9]
zipWith zip [[1 2] [1 2]] [[3 4] [3 4]]    == [[[1 3] [2 4]] [[1 3] [2 4]]]
```

### cat

Concatenates a row of rows into a single row.

```sire
cat [[1 2] [3 4] [5]]    == [1 2 3 4 5]
cat [[] [1 2] [3]]       == [1 2 3]
cat []                   == []
```

### catMap

Applies a function to each element of a row and concatenates the results.

```sire
catMap (rep 2) [1 2 3]    == [2 2 2 2 2 2]
catMap id [[1 2] [3 4]]   == [1 2 3 4]
catMap (mul 2) [1 2 3]    == []
```

### take

Returns the first n elements of a row.

```sire
take 3 [1 2 3 4 5]    == [1 2 3]
take 2 [b#a b#b]      == [b#a b#b]
take 5 [1 2 3]        == [1 2 3]
```

### drop

Removes the first n elements from a row.

```sire
drop 2 [1 2 3 4 5]          == [3 4 5]
drop 3 [b#a b#b b#c b#d]    == [b#d]
drop 5 [1 2 3]              == []
```

### rev

Reverses the order of elements in a row.

```sire
rev [1 2 3 4 5]          == [5 4 3 2 1]
rev [b#a b#b b#c b#d]    == [b#d b#c b#b b#a]
rev []                   == []
```

### span

Splits a row into two parts: the longest prefix that satisfies a predicate and the rest.

```sire
span (gte 3) [1 2 3 4 1 2 3 4]    == [[1 2 3] [4 1 2 3 4]]
span even [2 4 6 7 8 9]           == ([2 4 6], [7 8 9])
span FALSE [1 2 3]                == [[], [1 2 3]]
```

### splitAt

Splits a row at a given index.

```sire
splitAt 3 [1 2 3 4 5]    == [[1 2 3] [4 5]]
splitAt 0 [1 2 3]        == [[] [1 2 3]]
splitAt 5 [1 2 3]        == [[1 2 3] []]
```

### intersperse

Intersperses an element between every element of a row.

```sire
intersperse 0 [1 2 3]    == [1 0 2 0 3]
intersperse b#a [b#b]    == [b#b]
intersperse 0 []         == []
```

### insert

Inserts an element at a specified index in a row.

```sire
insert 1 b#x [b#a b#b b#c]    == [b#a b#x b#b b#c]
insert 0 0 [1 2 3]            == [0 1 2 3]
insert 3 4 [1 2 3]            == [1 2 3 4]
```

## Sorting and Filtering

### sort

Sorts a row in ascending order.

```sire
sort [3 1 4 1 5]      == [1 1 3 4 5]
sort [b#c b#a b#b]    == [b#a b#b b#c]
sort []               == []
```

### sortBy

Sorts a row using a custom comparison function.

```sire
sortBy (flip cmp) [1 3 2]      == [3 2 1]
sortBy (cmp) [[1 2] [] [1]]    == [[] [1] [1 2]]
```

### sortOn

Sorts a row by applying a function to each element before comparing.

```sire
sortOn (even) [1 3 2]             == [3 1 2]
sortOn len [[1 2] [3] [4 5 6]]    == [[3] [1 2] [4 5 6]]
```

### sortUniq

Sorts a row and removes duplicate elements.

```sire
sortUniq [3 1 4 1 5 3]    == [1 3 4 5]
sortUniq [b#a b#b b#a]    == [b#a b#b]
sortUniq []               == []
```

### filter

Keeps only the elements of a row that satisfy a predicate.

```sire
filter even [1 2 3 4 5]               == [2 4]
filter (neq b#a) [b#a b#b b#a b#c]    == [b#b b#c]
filter (const TRUE) [1 2 3]           == [1 2 3]
```

### delete

Removes all occurrences of a value from a row.

```sire
delete 3 [1 2 3 4 3 5]      == [1 2 4 5]
delete b#a [b#a b#b b#a]    == [b#b]
delete 42 [1 2 3]           == [1 2 3]
```

### findIdxMany

Finds all indices where a predicate is satisfied.

```sire
findIdxMany even [1 2 3 4 5 6]         == [1 [3 [5 0]]]
findIdxMany (eql b#a) [b#a b#b b#a]    == [0 [2 0]]
findIdxMany (const FALSE) [1 2 3]      == 0
```

### elemIdxMany

Finds all indices where a specific element occurs.

```sire
elemIdxMany 3 [1 2 3 4 3 5]      == [2 [4 0]]
elemIdxMany b#a [b#a b#b b#a]    == [0 [2 0]]
elemIdxMany 42 [1 2 3]           == 0
```

