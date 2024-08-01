# Chapter 2: Bit

## Sire Module: sire\_02\_bit

### Module Description

The `sire_02_bit` module defines foundational operations on bits (booleans) in the Sire language. It provides essential logical operations that serve as building blocks for more complex boolean logic and control flow structures.

Note: PLAN is an acronym for the four basic data structures used in this system: Pins, Laws, Apps, and Nats.

### Naming Conventions

Functions prefixed with an underscore (e.g., `_If`, `_Not`) are the primary definitions. Aliases without underscores (e.g., `if`, `not`) are provided for more readable code in typical use cases.

### Constants

| Name    | Value | Description              |
| ------- | ----- | ------------------------ |
| `TRUE`  | 1     | Represents logical true  |
| `FALSE` | 0     | Represents logical false |

### Core Functions

| Function       | Description              | Example              |
| -------------- | ------------------------ | -------------------- |
| `_If(x, t, e)` | Conditional operation    | See example below    |
| `_Not(x)`      | Logical NOT              | `_Not(TRUE)`         |
| `_Bit(x)`      | Converts to bit (0 or 1) | `_Bit(someValue)`    |
| `_And(x, y)`   | Logical AND              | `_And(TRUE, FALSE)`  |
| `_Or(x, y)`    | Logical OR               | `_Or(FALSE, TRUE)`   |
| `_Xor(x, y)`   | Logical XOR              | `_Xor(TRUE, FALSE)`  |
| `_Nand(x, y)`  | Logical NAND             | `_Nand(TRUE, TRUE)`  |
| `_Nor(x, y)`   | Logical NOR              | `_Nor(FALSE, FALSE)` |
| `_Xnor(x, y)`  | Logical XNOR             | `_Xnor(TRUE, TRUE)`  |

Example using vertical layout with pipe operator:

```sire
result
| _If (_And x y)
| _Or a b
| _Not z
```

### Aliases

The module provides aliases for all core functions, offering more idiomatic names for common usage.

| Alias  | Original Function |
| ------ | ----------------- |
| `if`   | `_If`             |
| `not`  | `_Not`            |
| `bit`  | `_Bit`            |
| `and`  | `_And`            |
| `or`   | `_Or`             |
| `xor`  | `_Xor`            |
| `nand` | `_Nand`           |
| `nor`  | `_Nor`            |
| `xnor` | `_Xnor`           |

Example of alias usage with vertical layout:

```sire
result
| if (and x y)
| or a b
| not z
```

### Additional Syntax

| Function         | Description                        | Example           |
| ---------------- | ---------------------------------- | ----------------- |
| `ifNot(x, t, e)` | Inverted conditional               | See example below |
| `else(x)`        | Identity function for else clauses | See example below |

Example using `ifNot` and `else`:

```sire
result
| ifNot condition
| thenExpr
| else elseExpr
```

Note: `ifNot` and `else` are marked as "always inline" and will not appear in the resulting PLAN code. They exist only to make code look nicer.

### Jetting

All jets in this module are optional. If implementing a subset of these jets, `_If` is the highest leverage, followed by `_Bit` and `_Not`.

#### The `_If` Jet

`_If` is particularly important because it can avoid creating thunks for both branches when the result is known to be demanded. An optimized implementation can:

1. Evaluate the conditional expression.
2. Choose a branch.
3. Evaluate the expression for that branch.

This optimization can significantly improve performance for conditional expressions with complex branch expressions.

Example of a complex conditional that benefits from `_If` optimization:

```sire
result
| _If (complexCondition x y)
| expensiveComputation a b
| anotherExpensiveComputation c d
```
