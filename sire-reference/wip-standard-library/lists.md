---
hidden: true
---

# Lists

Lists are zero-terminated, nested row 2-tuples. They are declared by prepending a `~` to what looks like row syntax, like this: `~[]` (in the REPL we have to wrap this in parentheses):

### NIL

Represents an empty list.

```sire
NIL                                         ; Returns 0 (the empty list)
listCase NIL 'empty' (\_ _ -> 'not empty')  ; Returns 'empty'
listFromRow [] == NIL                       ; Returns TRUE
```

### CONS

Constructs a new list by adding an element to the front of an existing list.

```sire
CONS 1 NIL                    ; Returns [1 0] (a list with one element)
CONS 'a' (CONS 'b' NIL)       ; Returns ['a' ['b' 0]] (a list with two elements)
CONS 1 (CONS 2 (CONS 3 NIL))  ; Returns [1 [2 [3 0]]] (a list with three elements)
```

### listCase

Pattern matches on a list, providing cases for empty and non-empty lists.

```sire
listCase NIL 'empty' (\x xs -> 'not empty')           ; Returns 'empty'
listCase (CONS 1 NIL) 'empty' (\x xs -> x)            ; Returns 1
listCase (CONS 1 (CONS 2 NIL)) 'empty' (\x xs -> xs)  ; Returns [2 0]
```

### listSing

Creates a singleton list containing one element.

```sire
listSing 5        ; Returns [5 0]
listSing 'hello'  ; Returns ['hello' 0]
listSing []       ; Returns [[] 0]
```

### listMap

Applies a function to every element of a list.

```sire
listMap (mul 2) ~[1 2 3]            ; Returns [2 [4 [6 0]]]
listMap show (CONS 1 (CONS 2 NIL))  ; Returns ["1" ["2" 0]]
listMap id NIL                      ; Returns NIL
```

### listForEach

Alias for `listMap`. Applies a function to every element of a list.

```sire
listForEach (* 2) (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns [2 [4 [6 0]]]
listForEach show (CONS 1 (CONS 2 NIL))            ; Returns ["1" ["2" 0]]
listForEach id NIL                                ; Returns NIL
```

### listHead

Returns the first element of a list wrapped in a Maybe, or NONE if the list is empty.

```sire
listHead (CONS 1 (CONS 2 NIL))  ; Returns SOME 1
listHead (CONS 'a' NIL)         ; Returns SOME 'a'
listHead NIL                    ; Returns NONE
```

### listSafeHead

Returns the first element of a list, or a fallback value if the list is empty.

```sire
listSafeHead 0 (CONS 1 (CONS 2 NIL))  ; Returns 1
listSafeHead 'x' (CONS 'a' NIL)       ; Returns 'a'
listSafeHead 'default' NIL            ; Returns 'default'
```

### listUnsafeHead

Returns the first element of a list. Unsafe if the list is empty.

```sire
listUnsafeHead (CONS 1 (CONS 2 NIL))  ; Returns 1
listUnsafeHead (CONS 'a' NIL)         ; Returns 'a'
listUnsafeHead NIL                    ; Undefined behavior
```

### listUnsafeTail

Returns the tail of a list (all elements except the first). Unsafe if the list is empty.

```sire
listUnsafeTail (CONS 1 (CONS 2 NIL))  ; Returns [2 0]
listUnsafeTail (CONS 'a' NIL)         ; Returns NIL
listUnsafeTail NIL                    ; Undefined behavior
```

### listIdxCps

Continuation-passing style function to get the element at a given index in a list.

```sire
listIdxCps 1 (CONS 'a' (CONS 'b' NIL)) 'not found' id  ; Returns 'b'
listIdxCps 0 (CONS 1 NIL) 'not found' id               ; Returns 1
listIdxCps 2 (CONS 1 (CONS 2 NIL)) 'not found' id      ; Returns 'not found'
```

### listIdxOr

Returns the element at a given index in a list, or a fallback value if the index is out of bounds.

```sire
listIdxOr 0 1 (CONS 'a' (CONS 'b' NIL))     ; Returns 'a'
listIdxOr 99 'z' (CONS 'a' (CONS 'b' NIL))  ; Returns 'z'
listIdxOr 0 'default' NIL                   ; Returns 'default'
```

### listIdx

Returns the element at a given index in a list, or 0 if the index is out of bounds.

```sire
listIdx 1 (CONS 'a' (CONS 'b' NIL))  ; Returns 'b'
listIdx 0 (CONS 1 NIL)               ; Returns 1
listIdx 2 (CONS 1 (CONS 2 NIL))      ; Returns 0
```

### listLastOr

Returns the last element of a list, or a fallback value if the list is empty.

```sire
listLastOr 0 (CONS 1 (CONS 2 NIL))  ; Returns 2
listLastOr 'z' (CONS 'a' NIL)       ; Returns 'a'
listLastOr 'default' NIL            ; Returns 'default'
```

### listUnsafeLast

Returns the last element of a list. Unsafe if the list is empty.

```sire
listUnsafeLast (CONS 1 (CONS 2 NIL))  ; Returns 2
listUnsafeLast (CONS 'a' NIL)         ; Returns 'a'
listUnsafeLast NIL                    ; Undefined behavior
```

### listLast

Returns the last element of a list wrapped in a Maybe, or NONE if the list is empty.

```sire
listLast (CONS 1 (CONS 2 NIL))  ; Returns SOME 2
listLast (CONS 'a' NIL)         ; Returns SOME 'a'
listLast NIL                    ; Returns NONE
```

### listFoldl

Left-associative fold of a list.

```sire
listFoldl (+) 0 (CONS 1 (CONS 2 (CONS 3 NIL)))              ; Returns 6
listFoldl (^) "" (CONS "a" (CONS "b" (CONS "c" NIL)))       ; Returns "abc"
listFoldl (\acc x -> CONS x acc) NIL (CONS 1 (CONS 2 NIL))  ; Returns [2 [1 0]]
```

### listFoldl1

Left-associative fold of a non-empty list, using the first element as the initial accumulator.

```sire
listFoldl1 (-) (CONS 10 (CONS 5 (CONS 3 NIL)))       ; Returns 2
listFoldl1 max (CONS 1 (CONS 5 (CONS 3 NIL)))        ; Returns 5
listFoldl1 (^) (CONS "a" (CONS "b" (CONS "c" NIL)))  ; Returns "abc"
```

### listFoldr

Right-associative fold of a list.

```sire
listFoldr (-) 0 (CONS 1 (CONS 2 (CONS 3 NIL)))         ; Returns 1 - (2 - (3 - 0)) = 2
listFoldr (^) "" (CONS "a" (CONS "b" (CONS "c" NIL)))  ; Returns "abc"
listFoldr CONS NIL (CONS 1 (CONS 2 NIL))               ; Returns [1 [2 0]]
```

### listLen

Computes the length of a list.

```sire
listLen (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns 3
listLen (CONS 'a' NIL)                  ; Returns 1
listLen NIL                             ; Returns 0
```

### listToRow

Converts a list to a row.

```sire
listToRow (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns [1 2 3]
listToRow (CONS 'a' (CONS 'b' NIL))       ; Returns ['a' 'b']
listToRow NIL                             ; Returns []
```

### sizedListToRow

Converts a list to a row of a specified size, padding with zeros if necessary.

```sire
sizedListToRow 3 (CONS 1 (CONS 2 NIL))           ; Returns [1 2 0]
sizedListToRow 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns [1 2]
sizedListToRow 4 NIL                             ; Returns [0 0 0 0]
```

### sizedListToRowRev

Converts a list to a row of a specified size in reverse order, padding with zeros if necessary.

```sire
sizedListToRowRev 3 (CONS 1 (CONS 2 NIL))           ; Returns [0 2 1]
sizedListToRowRev 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns [2 1]
sizedListToRowRev 4 NIL                             ; Returns [0 0 0 0]
```

### listToRowRev

Converts a list to a row in reverse order.

```sire
listToRowRev (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns [3 2 1]
listToRowRev (CONS 'a' (CONS 'b' NIL))       ; Returns ['b' 'a']
listToRowRev NIL                             ; Returns []
```

### listFromRow

Converts a row to a list.

```sire
listFromRow [1 2 3]    ; Returns CONS 1 (CONS 2 (CONS 3 NIL))
listFromRow ['a' 'b']  ; Returns CONS 'a' (CONS 'b' NIL)
listFromRow []         ; Returns NIL
```

### listAnd

Returns TRUE if all elements in the list are TRUE, otherwise FALSE.

```sire
listAnd (CONS TRUE (CONS TRUE NIL))   ; Returns TRUE
listAnd (CONS TRUE (CONS FALSE NIL))  ; Returns FALSE
listAnd NIL                           ; Returns TRUE
```

### listOr

Returns TRUE if any element in the list is TRUE, otherwise FALSE.

```sire
listOr (CONS FALSE (CONS TRUE NIL))   ; Returns TRUE
listOr (CONS FALSE (CONS FALSE NIL))  ; Returns FALSE
listOr NIL                            ; Returns FALSE
```

### listSum

Computes the sum of all elements in a list of numbers.

```sire
listSum (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns 6
listSum (CONS (-1) (CONS 1 NIL))        ; Returns 0
listSum NIL                             ; Returns 0
```

### listAll

Returns TRUE if all elements in the list satisfy the given predicate.

```sire
listAll even (CONS 2 (CONS 4 (CONS 6 NIL)))      ; Returns TRUE
listAll (> 0) (CONS 1 (CONS 2 (CONS (-1) NIL)))  ; Returns FALSE
listAll id NIL                                   ; Returns TRUE
```

### listAllEql

Returns TRUE if all elements in the list are equal.

```sire
listAllEql (CONS 1 (CONS 1 (CONS 1 NIL)))  ; Returns TRUE
listAllEql (CONS 'a' (CONS 'a' NIL))       ; Returns TRUE
listAllEql (CONS 1 (CONS 2 NIL))           ; Returns FALSE
```

### listAny

Returns TRUE if any element in the list satisfies the given predicate.

```sire
listAny odd (CONS 2 (CONS 3 (CONS 4 NIL)))    ; Returns TRUE
listAny (< 0) (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns FALSE
listAny id NIL                                ; Returns FALSE
```

### listHas

Checks if a list contains a specific element.

```sire
listHas 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns TRUE
listHas 'a' (CONS 'b' (CONS 'c' NIL))     ; Returns FALSE
listHas 1 NIL                             ; Returns FALSE
```

### listEnumFrom

Creates an infinite list of consecutive integers starting from a given number.

```sire
take 5 (listEnumFrom 1)               ; Returns CONS 1 (CONS 2 (CONS 3 (CONS 4 (CONS 5 NIL))))
listHead (listEnumFrom 10)            ; Returns SOME 10
listHead (listTail (listEnumFrom 7))  ; Returns SOME 8
```

### listWeld

Concatenates two lists.

```sire
listWeld (CONS 1 (CONS 2 NIL)) (CONS 3 (CONS 4 NIL))  ; Returns CONS 1 (CONS 2 (CONS 3 (CONS 4 NIL)))
listWeld (CONS 'a' NIL) (CONS 'b' NIL)                ; Returns CONS 'a' (CONS 'b' NIL)
listWeld NIL (CONS 1 NIL)                             ; Returns CONS 1 NIL
```

### listCat

Concatenates a list of lists into a single list.

```sire
listCat (CONS (CONS 1 (CONS 2 NIL)) (CONS (CONS 3 NIL) NIL))        ; Returns CONS 1 (CONS 2 (CONS 3 NIL))
listCat (CONS (CONS 'a' NIL) (CONS (CONS 'b' (CONS 'c' NIL)) NIL))  ; Returns CONS 'a' (CONS 'b' (CONS 'c' NIL))
listCat (CONS NIL (CONS NIL NIL))                                   ; Returns NIL
```

### listCatMap

Applies a function to all elements in a list and concatenates the results.

```sire
listCatMap (\x -> CONS x (CONS x NIL)) (CONS 1 (CONS 2 NIL))  ; Returns CONS 1 (CONS 1 (CONS 2 (CONS 2 NIL)))
listCatMap (\x -> CONS (x + 1) NIL) (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns CONS 2 (CONS 3 (CONS 4 NIL))
listCatMap (const NIL) (CONS 1 (CONS 2 NIL))  ; Returns NIL
```

### listTake

Returns the first n elements of a list.

```sire
listTake 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns CONS 1 (CONS 2 NIL)
listTake 5 (CONS 'a' (CONS 'b' NIL))       ; Returns CONS 'a' (CONS 'b' NIL)
listTake 0 (CONS 1 (CONS 2 NIL))           ; Returns NIL
```

### listDrop

Drops the first n elements of a list and returns the rest.

```sire
listDrop 2 (CONS 1 (CONS 2 (CONS 3 NIL)))  ; Returns CONS 3 NIL
listDrop 1 (CONS 'a' (CONS 'b' NIL))       ; Returns CONS 'b' NIL
listDrop 5 (CONS 1 (CONS 2 NIL))           ; Returns NIL
```

### listTakeWhile

Takes elements from a list while a predicate holds.

```sire
listTakeWhile (< 3) (CONS 1 (CONS 2 (CONS 3 (CONS 4 NIL))))  ; Returns CONS 1 (CONS 2 NIL)
listTakeWhile isLower (CONS 'a' (CONS 'b' (CONS 'C' (CONS 'd' NIL))))  ; Returns CONS 'a' (CONS 'b' NIL)
listTakeWhile (const FALSE) (CONS 1 (CONS 2 NIL))  ; Returns NIL
```

### listDropWhile

Drops elements from the beginning of a list while a predicate function returns true.

This function takes a predicate function and a list as input. It removes elements from the front of the list as long as the predicate function returns true for each element. Once an element is encountered for which the predicate returns false, the function returns the remaining list.

```sire
listDropWhile (lth 3) ~[1 2 3 4 5]  ; Returns ~[3 4 5]
listDropWhile isEven ~[2 4 6 7 8 9] ; Returns ~[7 8 9]
listDropWhile (const FALSE) ~[1 2 3] ; Returns ~[1 2 3]
```

### listTakeWhile

Takes elements from the beginning of a list while a predicate function returns true.

```sire
listTakeWhile (lth 3) ~[1 2 3 4 5]  ; Returns ~[1 2]
listTakeWhile isEven ~[2 4 6 7 8 9] ; Returns ~[2 4 6]
listTakeWhile (const TRUE) ~[1 2 3] ; Returns ~[1 2 3]
```

### sizedListToRow

Converts a list to a row of a specified size, padding with zeros if necessary.

```sire
sizedListToRow 3 ~[1 2 3 4 5] ; Returns [1 2 3]
sizedListToRow 5 ~[1 2 3]     ; Returns [1 2 3 0 0]
sizedListToRow 0 ~[1 2 3]     ; Returns []
```

### sizedListToRowRev

Converts a list to a reversed row of a specified size, padding with zeros if necessary.

```sire
sizedListToRowRev 3 ~[1 2 3 4 5] ; Returns [3 2 1]
sizedListToRowRev 5 ~[1 2 3]     ; Returns [3 2 1 0 0]
sizedListToRowRev 0 ~[1 2 3]     ; Returns []
```

### listToRow

Converts a list to a row.

```sire
listToRow ~[1 2 3]     ; Returns [1 2 3]
listToRow ~[]          ; Returns []
listToRow ~[1 [2 3] 4] ; Returns [1 [2 3] 4]
```

### listToRowRev

Converts a list to a reversed row.

```sire
listToRowRev ~[1 2 3]     ; Returns [3 2 1]
listToRowRev ~[]          ; Returns []
listToRowRev ~[1 [2 3] 4] ; Returns [4 [2 3] 1]
```

### listRev

Reverses a list.

```sire
listRev ~[1 2 3]     ; Returns ~[3 2 1]
listRev ~[]          ; Returns ~[]
listRev ~[1 [2 3] 4] ; Returns ~[4 [2 3] 1]
```

### listSnoc

Appends an element to the end of a list.

```sire
listSnoc ~[1 2] 3      ; Returns ~[1 2 3]
listSnoc ~[] 1         ; Returns ~[1]
listSnoc ~[1 [2 3]] 4  ; Returns ~[1 [2 3] 4]
```

### listProd

Computes the Cartesian product of two lists.

```sire
listProd ~[1 2] ~[3 4]    ; Returns ~[[1 3] [1 4] [2 3] [2 4]]
listProd ~[1] ~[2 3 4]    ; Returns ~[[1 2] [1 3] [1 4]]
listProd ~[] ~[1 2]       ; Returns ~[]
```
