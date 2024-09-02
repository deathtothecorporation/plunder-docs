# KV Tables

A tab is a data-jetted map from noun to noun.

### tabSing

Creates a singleton table with one key-value pair.

```sire
tabSing 1 2        == [1=2]
tabSing {a} {b}    == [a=(%b)]
tabSing 0 []       == [0=[]]
```

### isTab

Checks if the given value is a table.

```sire
isTab #[]           == 1
isTab #[x=3 y=4]    == 1
isTab []            == 0
```

### tabSearchCase

Inline function that finds the index of a key within a table. Returns a continuation with the associated value.

```sire
tabSearchCase 1 #[1=3 2=4] {nf} (ix v & [ix v])    == [0 3]
tabSearchCase 2 #[1=3 2=4] {nf} (ix v & [ix v])    == [1 4]
tabSearchCase 3 #[1=3 2=4] {nf} (ix v & [ix v])    == %nf
```

### tabLen

Returns the number of key-value pairs in a table.

```sire
tabLen #[]           == 0
tabLen #[x=0]        == 1
tabLen #[x=0 y=1]    == 2
```

### tabIdx

Retrieves the value associated with a given key in a table.

```sire
tabIdx 0 #[0=8]             == 8
tabIdx %aa #(bb=8, aa=9)    == 9
tabIdx 3 #[1=4 2=5]         == 0
```

### tabKeysSet

Returns the set of keys in a table.

```sire
tabKeysSet #[1=3 2=4]    == %[1 2]
tabKeysSet #[]           == %[]
tabKeysSet #[a=1 b=2]    == %[a b]
```

### tabKeysRow

Returns the row of keys in a table.

```sire
tabKeysRow #[3=3 4=4]    == [3 4]
tabKeysRow #[]           == []
tabKeysRow #[a=1 b=2]    == [%a %b]
```

### tabKeysList

Returns the list of keys in a table.

```sire
tabKeysList #[3=3 4=4]    == [3 [4 0]]
tabKeysList #[]           == 0
tabKeysList #[a=1 b=2]    == [%a [%b 0]]
```

### tabHas

Checks if a key exists in a table.

```sire
tabHas %aa #(bb=8, aa=9)    == 1
tabHas %cc #(bb=8, aa=9)    == 0
tabHas 1 #[1=4 2=5]         == 1
```

### tabGet

Retrieves the value associated with a given key in a table.

```sire
tabGet #[1=4 2=5] 1    == 4
tabGet #[1=4 2=5] 2    == 5
tabGet #[1=4 2=5] 3    == 0
```

### tabValsRow

Returns the row of values in a table.

```sire
tabValsRow #[3=3 4=4]    == [3 4]
tabValsRow #[]           == []
tabValsRow #[a=1 b=2]    == [1 2]
```

### tabValsList

Returns the list of values in a table.

```sire
tabValsList #[3=3 4=4]    == [3 [4 0]]
tabValsList #[]           == 0
tabValsList #[a=1 b=2]    == [1 [2 0]]
```

### tabSwitch

Looks up a key in a table, returning a default value if not found.

```sire
tabSwitch 1 {not found} #[1=3 2=4]    == 3
tabSwitch 2 {not found} #[1=3 2=4]    == 4
tabSwitch 3 {not found} #[1=3 2=4]    == 0
```

### tabFromPairs

Creates a table from a row of key-value pairs.

```sire
tabFromPairs [[3 8] [5 9]]           == [3=8 5=9]
tabFromPairs [[5 9] [3 8]]           == [3=8 5=9]
tabFromPairs [[3 7] [3 8] [5 9]]     == [3=8 5=9]
```

### tabFromAscPairs

Creates a table from an ascending row of key-value pairs.

```sire
tabFromAscPairs [[1 4] [2 5] [3 6]]    == [1=4 2=5 3=6]
tabFromAscPairs []                     == []
tabFromAscPairs [[a 1] [b 2]]          == [a=1 b=2]
```

### tabToPairs

Converts a table to a row of key-value pairs.

```sire
tabToPairs #[1=4 2=5 3=6]    == [[1 4] [2 5] [3 6]]
tabToPairs #[]               == []
tabToPairs #[a=1 b=2]        == [[a 1] [b 2]]
```

### tabToPairList

Converts a table to a list of key-value pairs.

```sire
tabToPairList #[3=8 5=9]    == [[3 8] [[5 9] 0]]
tabToPairList #[]           == 0
tabToPairList #[a=1]        == [[%a 1] 0]
```

### tabToList

Converts a table to a list of key-value pairs.

```sire
tabToList #[3=8 5=9]    == [[3 8] [5 9] 0]
tabToList #[]           == 0
tabToList #[a=1 b=2]    == [[%a 1] [%b 2] 0]
```

### tabPut

Inserts or updates a key-value pair in a table.

```sire
tabPut #[1=3 2=4] 1 5    == [1=5 2=4]
tabPut #[1=3 2=4] 3 5    == [1=3 2=4 3=5]
tabPut #[] 1 5           == [1=5]
```

### tabFromPairsList

Creates a table from a list of key-value pairs.

```sire
tabFromPairsList ~[[3 8] [5 9]]      == [3=8 5=9]
tabFromPairsList 0                   == []
tabFromPairsList ~[[%a 1] [%b 2]]    == [a=1 b=2]
```

### tabIns

Inserts a key-value pair into a table.

```sire
tabIns 3 3 #[3=4]    == [3=3]
tabIns 4 4 #[3=4]    == [3=4 4=4]
tabIns 0 0 #[]       == [0=0]
```

### tabIsEmpty

Checks if a table is empty.

```sire
tabIsEmpty #[]           == 1
tabIsEmpty #[1=2]        == 0
tabIsEmpty #[a=1 b=2]    == 0
```

### tabDel

Deletes a key-value pair from a table.

```sire
tabDel 1 #[1=3 2=4]     == [2=4]
tabDel 3 #[1=3 2=4]     == [1=3 2=4]
tabDel %a #[a=1 b=2]    == [b=2]
```

### tabPop

Removes and returns the first key-value pair from a table along with the remaining table.

```sire
tabPop #[1=3 2=4]        == [1 3 [2=4]]
tabPop #[a=1]            == [%a 1 #[]]
tabPop #[a=1 b=2 c=3]    == [%a 1 [b=2 c=3]]
```

### tabSplitAt

Splits a table into two tables at a given index.

```sire
tabSplitAt 1 #[1=3 2=4 3=5]    == [[1=3] [2=4 3=5]]
tabSplitAt 0 #[1=3 2=4]        == [#[] [1=3 2=4]]
tabSplitAt 2 #[a=1 b=2 c=3]    == [[a=1 b=2] [c=3]]
```

### tabSplitLT

Splits a table into two tables based on a key.

```sire
tabSplitLT 2 #[1=3 2=4 3=5]     == [[1=3] [2=4 3=5]]
tabSplitLT 0 #[1=3 2=4]         == [#[] [1=3 2=4]]
tabSplitLT %b #[a=1 b=2 c=3]    == [[a=1] [b=2 c=3]]
```

### tabAlter

Applies a function to the value associated with a key, potentially inserting or deleting the key-value pair.

```sire
tabAlter (v & SOME (inc v)) 1 #[1=3 2=4]    == [1=1 2=4]
tabAlter (v & NONE) 1 #[1=3 2=4]            == [2=4]
tabAlter (v & SOME 5) 3 #[1=3 2=4]          == [1=3 2=4 3=5]
```

### tabMapWithKey

Applies a function to each key-value pair in a table.

```sire
tabMapWithKey (k v & add k v) #[1=3 2=4]    == [1=4 2=6]
tabMapWithKey (k v & [k v]) #[a=1 b=2]      == [a=[%a 1] b=[%b 2]]
tabMapWithKey (k v & inc v) #[]             == #[]
```

### tabMap

Applies a function to each value in a table.

```sire
tabMap inc #[1=3 2=4]       == [1=4 2=5]
tabMap (mul 2) #[a=1 b=2]   == [a=2 b=4]
tabMap id #[]               == #[]
```

### tabUnionWith

Merges two tables, using a function to resolve conflicts.

```sire
tabUnionWith add #[1=1 2=2] #[2=20 3=30]           == [1=1 2=22 3=30]
tabUnionWith const #[a=1 b=2] #[b=20 c=30]         == [a=1 b=2 c=30]
tabUnionWith (x y & [x y]) #[1=10] #[1=11 2=12]    == [1=[10 11] 2=12]
```

### tabUnion

Merges two tables, with left-biased conflict resolution.

```sire
tabUnion #[1=1 2=2] #[2=20 3=30]    == [1=1 2=2 3=30]
tabUnion #[a=1 b=2] #[b=20 c=30]    == [a=1 b=2 c=30]
tabUnion #[] #[1=10 2=20]           == [1=10 2=20]
```

### tabWeld

Alias for tabUnion. Merges two tables, with left-biased conflict resolution.

```sire
tabWeld #[1=1 2=2] #[2=20 3=30]    == [1=1 2=2 3=30]
tabWeld #[a=1 b=2] #[b=20 c=30]    == [a=1 b=2 c=30]
tabWeld #[] #[1=10 2=20]           == [1=10 2=20]
```

### tabCatRow

Merges a row of tables into a single table, with left-biased conflict resolution.

```sire
tabCatRow [#[1=1] #[2=2] #[3=3]]           == [1=1 2=2 3=3]
tabCatRow [#[a=1 b=2] #[b=20 c=30] #[]]    == [a=1 b=2 c=30]
tabCatRow []                               == #[]
```

### tabLookup

Looks up a key in a table, returning a Maybe value.

```sire
tabLookup 1 #[1=3 2=4]    == (0 3) ; SOME
tabLookup 2 #[1=3 2=4]    == (0 4) ; SOME
tabLookup 3 #[1=3 2=4]    == 0 ; NONE
```

### tabMinKey

Returns the smallest key in a table.

```sire
tabMinKey #[3=30 1=10 2=20]    == 1
tabMinKey #[a=1 b=2 c=3]       == %a
tabMinKey #[z=26]              == %z
```

### tabFoldlWithKey

Folds over a table, applying a function to each key-value pair and an accumulator.

```sire
tabFoldlWithKey (acc k v & add acc (mul k v)) 0 #[1=3 2=4]    == 11
tabFoldlWithKey (acc k v & weld acc [k v]) [] #[a=1 b=2]      == [%a 1 %b 2]
tabFoldlWithKey (acc k v & inc acc) 0 #[]                     == 0
```

### tabElemIdx

Returns the index of a key-value pair in a table, treating it as an array.

```sire
tabElemIdx 0 #[a=1 b=2 c=3]    == [%a 1]
tabElemIdx 1 #[a=1 b=2 c=3]    == [%b 2]
tabElemIdx 3 #[a=1 b=2 c=3]    == 0
```

### emptyTab

An empty table constant.

```sire
emptyTab                    == #[]
tabIsEmpty emptyTab         == 1
tabUnion emptyTab #[1=2]    == [1=2]
```

### tabInsWith

Inserts a key-value pair into a table, using a function to combine values if the key already exists.

```sire
tabInsWith add 1 6 #[1=3 2=4]          == [1=9 2=4]
tabInsWith const 1 6 #[1=3 2=4]        == [1=6 2=4]
tabInsWith (x y & [x y]) 3 6 #[1=3]    == [1=3 3=6]
```

### tabFilterWithKey

Filters a table based on a predicate function applied to each key-value pair.

```sire
tabFilterWithKey (k v & even k) #[1=1 2=2 3=3 4=4]     == [2=2 4=4]
tabFilterWithKey (k v & eql k v) #[a=1 b=2 c=3]        == #[]
tabFilterWithKey (k v & gth v 2) #[a=1 b=2 c=3 d=4]    == [c=3 d=4]
```
