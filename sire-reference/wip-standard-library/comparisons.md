# Comparisons

### LT

Represents "less than" in ordering comparisons.

```sire
LT == 0
```

### EQ

Represents "equal to" in ordering comparisons.

```sire
EQ == 1
```

### GT

Represents "greater than" in ordering comparisons.

```sire
GT == 2
```

### ordWeld

Combines two ordering results, giving precedence to the first non-EQ result.

```sire
ordWeld LT EQ    == 0    ; LT
ordWeld EQ GT    == 2    ; GT
ordWeld EQ EQ    == 1    ; EQ
ordWeld LT GT    == 0    ; LT
```

### isZero

Checks if a value is zero.

```sire
isZero 0      == 1
isZero 1      == 0
isZero 100    == 0
```

### isOne

Checks if a value is one.

```sire
isOne 1      == 1
isOne 100    == 0
```

### cmp

Compares two values, returning LT, EQ, or GT.

```sire
cmp 1 2            == LT
cmp 2 2            == EQ
cmp 3 2            == GT
cmp [1 2] [1 3]    == LT
cmp [1 2] [1 2]    == EQ
```

### eql

Checks if two values are equal.

```sire
eql 1 1            == 1
eql 1 2            == 0
eql [1 2] [1 2]    == 1
eql [1 2] [1 3]    == 0
```

### neq

Checks if two values are not equal.

```sire
neq 1 2            == 1
neq 1 1            == 0
neq [1 2] [1 3]    == 1
neq [1 2] [1 2]    == 0
```

### lth

Checks if the first value is less than the second.

```sire
lth 1 2    == 1
lth 2 1    == 0
lth 1 1    == 0
```

### lte

Checks if the first value is less than or equal to the second.

```sire
lte 1 2    == 1
lte 1 1    == 1
lte 2 1    == 0
```

### gth

Checks if the first value is greater than the second.

```sire
gth 2 1    == 1
gth 1 2    == 0
gth 1 1    == 0
```

### gte

Checks if the first value is greater than or equal to the second.

```sire
gte 2 1    == 1
gte 1 1    == 1
gte 1 2    == 0
```

### min

Returns the minimum of two values.

```sire
min 1 2    == 1
min 2 1    == 1
min 1 1    == 1
```

### max

Returns the maximum of two values.

```sire
max 1 2    == 2
max 2 1    == 2
max 1 1    == 1
```
