# Chapter 1: Fan

## Sire Module: sire\_01\_fan

### Module Description

The `sire_01_fan` module provides wrappers around primitive PLAN operations and essential utility functions. It forms the basis for higher-level operations in Sire.

### Naming Conventions

Functions prefixed with an underscore (e.g., `_Force`, `_IsPin`) are the primary definitions. These names are used internally and in low-level operations. Aliases without underscores (e.g., `force`, `isPin`) are provided for more readable code in typical use cases.

### Primitive Wrappers

These functions wrap the fundamental operations of the PLAN

| Name      | Description                                                          |
| --------- | -------------------------------------------------------------------- |
| `LAW`     | Primitive operation 0. Represents a function definition.             |
| `valCase` | Primitive operation 1. Used for pattern matching on values.          |
| `natCase` | Primitive operation 2. Used for pattern matching on natural numbers. |
| `inc`     | Primitive operation 3. Increments a natural number.                  |
| `PIN`     | Primitive operation 4. Creates a pinned (immutable) value.           |
| `die`     | Crash operation. Halts execution with an error.                      |
| `todo`    | Crash operation. Indicates unimplemented functionality.              |

Example:

```sire
(LAW {add} 2 (valCase _))  ; Defines a function named 'add' with 2 arguments
```

### Core Functions

| Function           | Description                                | Example                                      |
| ------------------ | ------------------------------------------ | -------------------------------------------- |
| `_Force(x)`        | Forces evaluation of x                     | `_Force(lazyValue)`                          |
| `_Seq(x, y)`       | Sequentially evaluates x then y, returns y | `_Seq(sideEffect, result)`                   |
| `deepseq(x, y)`    | Deeply evaluates x, then evaluates y       | `deepseq(complexStructure, result)`          |
| `_Trace(x, y)`     | Traces x, then returns y                   | `_Trace(debugValue, actualResult)`           |
| `_DeepTrace(x, y)` | Deeply traces x, then returns y            | `_DeepTrace(complexStructure, actualResult)` |

Example:

```sire
result = _Seq(printDebugInfo(x), computeResult(x))
```

### Type Checking

| Function      | Description                     | Example                  |
| ------------- | ------------------------------- | ------------------------ |
| `_IsPin(x)`   | Checks if x is a PIN            | `_IsPin(myValue)`        |
| `_IsLaw(x)`   | Checks if x is a LAW            | `_IsLaw(myFunction)`     |
| `_IsApp(x)`   | Checks if x is an application   | `_IsApp(someExpression)` |
| `_IsNat(x)`   | Checks if x is a natural number | `_IsNat(42)`             |
| `_PlanTag(x)` | Returns the PLAN tag of x       | `_PlanTag(anyValue)`     |
| `_IsZero(x)`  | Checks if x is zero             | `_IsZero(count)`         |
| `_IsOne(x)`   | Checks if x is one              | `_IsOne(index)`          |

Example:

```sire
if (_IsNat(x) && !_IsZero(x)) {
    // x is a positive natural number
}
```

### Accessors

| Function      | Description                            | Example                 |
| ------------- | -------------------------------------- | ----------------------- |
| `_PinItem(x)` | Retrieves the item of a PIN            | `_PinItem(pinnedValue)` |
| `_LawName(x)` | Retrieves the name of a LAW            | `_LawName(myFunction)`  |
| `_LawArgs(x)` | Retrieves the arguments of a LAW       | `_LawArgs(myFunction)`  |
| `_LawBody(x)` | Retrieves the body of a LAW            | `_LawBody(myFunction)`  |
| `_Car(x)`     | Retrieves the first element of a pair  | `_Car(myPair)`          |
| `_Cdr(x)`     | Retrieves the second element of a pair | `_Cdr(myPair)`          |

Example:

```sire
functionName = _LawName(someFunction)
firstArg = _Car(argumentPair)
```

### Lisp-Style Accessors

`caar`, `cadr`, `cdar`, `cddr`, `caaar`, `caadr`, `cadar`, `caddr`, `cdaar`, `cdadr`, `cddar`, `cdddr`, `caaaar`

These functions provide shorthand for nested `_Car` and `_Cdr` operations on nested pairs.

Example:

```sire
thirdElement = caddr(myList)  ; Equivalent to _Car(_Cdr(_Cdr(myList)))
```

### Common Combinators

| Function           | Description                          | Example                                 |
| ------------------ | ------------------------------------ | --------------------------------------- |
| `id(x)`            | Identity function                    | `id(5) ; Returns 5`                     |
| `const(x, y)`      | Constant function, returns x         | `const(7, anyValue) ; Always returns 7` |
| `ignore(x, y)`     | Ignores first argument, returns y    | `ignore(sideEffect, result)`            |
| `compose(g, f, y)` | Function composition (g ∘ f)         | `compose(double, inc, 3) ; Returns 8`   |
| `flip(f, x, y)`    | Flips arguments of a binary function | `flip(subtract, 3, 7) ; Returns 4`      |
| `apply(f, x)`      | Applies function f to argument x     | `apply(double, 4) ; Returns 8`          |
| `supply(x, f)`     | Supplies argument x to function f    | `supply(5, double) ; Returns 10`        |

### Aliases

The module provides aliases for many of the underscore-prefixed functions. These aliases offer more idiomatic names for common usage, while the underscore versions remain available for low-level or specialized scenarios.

| Alias       | Original Function |
| ----------- | ----------------- |
| `force`     | `_Force`          |
| `seq`       | `_Seq`            |
| `trk`       | `_Trace`          |
| `trace`     | `_Trace`          |
| `dTrk`      | `_DeepTrace`      |
| `deepTrace` | `_DeepTrace`      |
| `isPin`     | `_IsPin`          |
| `isLaw`     | `_IsLaw`          |
| `isApp`     | `_IsApp`          |
| `isNat`     | `_IsNat`          |
| `planTag`   | `_PlanTag`        |
| `pinItem`   | `_PinItem`        |
| `lawName`   | `_LawName`        |
| `lawArgs`   | `_LawArgs`        |
| `lawBody`   | `_LawBody`        |
| `car`       | `_Car`            |
| `cdr`       | `_Cdr`            |
| `isZero`    | `_IsZero`         |
| `isOne`     | `_IsOne`          |

Example of alias usage:

```sire
if (isNat(x) && !isZero(x)) {
    // This is equivalent to the earlier example but uses aliases
}
```

Note: All jets in this module are optional. The `_Trk` jet is the only effectful jet, printing its first argument to a debug log.
