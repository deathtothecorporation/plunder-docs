# PLAN in Depth

## Introduction

This document provides an in-depth exploration of PLAN. We'll start by examining the fundamental opcodes and reduction rules, then delve into the language's unique characteristics and design philosophy.

## PLAN Opcodes

PLAN is built upon five fundamental opcodes, each represented by one of the first five natural numbers. These opcodes form the foundation of PLAN's computational model.

### 0: Law Construction

Syntax: `(0 n a b)`

This opcode constructs a new law (user-defined function) with name `n`, arity `a`, and body `b`. The resulting law is represented as `{n a b}`.

### 1: Pattern Matching

Syntax: `(1 p l a n x)`

This opcode implements pattern matching on the value `x` (is it a pin, a law, an app or a nat?):

```txt
case x of
    PIN <i>     -> p i
    LAW {n,a,b} -> l n a b
    APP (f x)   -> a f x
    NAT x       -> n x
```

### 2: Natural Number Pattern Matching

Syntax: `(2 z p x)`

This opcode implements pattern matching for natural numbers (is it zero or (n+1) ?):

```txt
case x of
    0 -> z
    x -> p (x-1)
```

### 3: Increment

Syntax: `(3 x)`

This opcode increments the natural number `x` by 1.

### 4: Pin Creation

Syntax: `(4 x)`

This opcode normalizes `x` and constructs a new pin `<x>`.

## PLAN Reduction Rules

The reduction rules in PLAN define how expressions are evaluated. These rules are applied repeatedly until no further reductions are possible, at which point the expression is in its normal form.

### Evaluation Function (E)

```txt
E(o:@)     = o
E(o:<x>)   = o
E(o:(f x)) =
    E(f)
    when A(f)=1
        o <- X(o,o)
        E(o)
    o
E(o:{n a b}) =
    if a!=0 then o else
        o <- <>
        o <- R(0,o,b)
        E(o)
```

### Normalization Function (F)

```txt
F(o) =
    E(o)
    when o:(f x)
         F(f); F(x)
    o
```

### Application (A)

```txt
A((f x))     = A(f)-1
A(<p>)       = A(p)
A({n a b})   = a
A(n:@)       = I(1, (3 5 3), n)
```

### Execution (X)

```txt
X((f x), e)         = X(f,e)
X(<p>, e)           = X(p,e)
X({n a b}, e)       = R(a,e,b)
X(0, (_ n a b))     = {N(n) N(a) F(b)}
X(1, (_ p l a n x)) = P(p,l,a,n,E(x))
X(2, (_ z p x))     = C(z,p,N(x))
X(3, (_ x))         = N(x)+1
X(4, (_ x))         = <F(x)>
```

These reduction rules, combined with the opcodes, define the operational semantics of PLAN. They determine how PLAN expressions are evaluated and how computation proceeds.

## The EDSL Within PLAN

PLAN contains an embedded domain-specific language (EDSL) for defining law bodies. This EDSL is not a separate language per se, but rather a specific way of using PLAN constructs to represent law bodies.

The EDSL for law bodies has the following constructs:

```txt
Law = Self                -- self-reference
    | Var Nat             -- reference bound name (using indices instead of names)
    | Const PLAN          -- produce constant value
    | (Law Law)           -- function application
    | let Law in Law      -- let binding (adding a value to the end of the environment. may self-reference)
```

These constructs are encoded within PLAN using partially applied forms of PLAN's native opcodes:

|---------|--------------------|
| pattern | meaning            |
|---------|--------------------|
| (0 f x) | (f x)              |
| (1 v b) | let v in b         |
| (2 k)   | Const k            |
| 0       | Self               |
| 1       | var 1              |
| 2       | var 2              |
| x       | Const x (fallback) |
|---------|--------------------|

It's important to note that none of these patterns reduce under normal PLAN evaluation rules. This allows any valid AST of the EDSL to be stored using this encoding.

The EDSL and PLAN proper are tightly intertwined:

1. Within a law body, you can always drop back into regular PLAN either by using the `Const` construct or automatically when a part of the law body doesn't have meaning in the EDSL.
2. Conversely, PLAN switches into the EDSL whenever a law has been applied to enough arguments.

This dual-language structure allows PLAN to maintain a simple core while providing more expressive power for defining functions.

## Reflexivity in PLAN

In PLAN, any program can deconstruct and analyze any terms it encounters during execution. This means that PLAN programs have the ability to:

1. Inspect the structure of data and code
2. Make decisions based on the form of expressions
3. Modify or generate new code dynamically

This level of reflexivity is not common in many programming languages or evaluation models.

## PLAN as a Comprehensive Data Structure

While it's not entirely accurate to call PLAN a format for human-readable executables or binaries, this perspective highlights PLAN's unique position as a comprehensive data structure that bridges multiple domains of computation and storage.

### Balancing Multiple Concerns

PLAN is designed to strike a balance between several aspects of computation and data representation:

1. **Human Readability**: Unlike typical low-level formats, PLAN maintains a degree of readability that allows developers to inspect and understand "binaries" directly.
2. **Functional Compile Target**: PLAN serves as an excellent target for functional language compilers, capturing complex semantics in a simple form.
3. **Efficient Memory Representation**: The structure of PLAN is designed to map well to in-memory data structures, allowing for efficient execution.
4. **Effective On-Disk Representation**: PLAN's format is also suitable for direct storage, enabling persistent storage without conversion.
5. **Closure Representation**: PLAN stores functions together with their environments, effectively representing closures and eliminating the need for free variables or assumed environments.

## Clarification on Syntax and Semantics

In discussions of PLAN, there may be some confusion regarding the syntax and semantics of certain constructs. It's important to clarify that:

- `{}` is used to denote a law (function definition) in PLAN. It's not a general-purpose list constructor.
- `()` is used for function application, but it's also used more generally to construct applications (which can be thought of as pairs).

Neither `{}` nor `()` should be thought of as general "lists of values". Instead:

- `{n a b}` represents a law with name `n`, arity `a`, and body `b`.
- `(f x)` represents the application of `f` to `x`, which could be function application if `f` is a function, or could just be a pair if `f` is not a function.
