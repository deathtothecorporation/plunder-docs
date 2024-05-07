# Explanation

## The E() Function

```
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

The `E()` function is responsible for evaluating an expression to its weak head normal form (WHNF). It takes a single parameter `o`, which represents the expression to be evaluated.

**Procedure:**

1. It checks the type of the input expression `o`:
   * If the type of `o` is `'hol'` (indicating a hole or an undefined value):
     * It raises an exception indicating a loop, as evaluating a hole is not allowed.
   * If the type of `o` is `'app'` (indicating an application):
     * It recursively calls `E()` on the head of the application (`o.head`) to evaluate the function to its WHNF.
     * It then checks if the arity of the evaluated function (`A(o.head)`) is equal to 1:
       * If the arity is 1, it means the function is saturated (has enough arguments).
       * It updates `o` by calling `S(o)` to simplify the expression by removing any unnecessary pins in the head of the application.
       * It then calls `X(o, o)` to perform one step of simplification on the saturated expression.
       * It updates `o` with the simplified expression.
       * It recursively calls `E()` on the updated `o` to continue the evaluation process.
2. Finally, it returns the evaluated expression `o`.

The `E()` function relies on the `A()` function to determine the arity of a function, the `S()` function to simplify the expression by removing unnecessary pins, and the `X()` function to perform the actual simplification step on a saturated expression.

The evaluation process continues recursively until the expression reaches its WHNF.

## The X() Function

```
X((f x), e)         = X(f,e)
X(<p>, e)           = X(p,e)
X({n a b}, e)       = R(a,e,b)
X(0, (_ n a b))     = {N(n) N(a) F(b)}
X(1, (_ p l a n x)) = P(p,l,a,n,E(x))
X(2, (_ z p x))     = C(z,p,N(x))
X(3, (_ x))         = N(x)+1
X(4, (_ x))         = <F(x)>
```

The `X()` function is responsible for performing one step of evaluation on a saturated expression.&#x20;

Inputs:

* `k`: The expression to be simplified.
* `e`: The environment.

**Procedure:**

* If the input is an app, pin, or law, `X()` applies the corresponding reduction rules based on the PLAN specification.
* If the input is a special operation, it executes the associated built-in routine.

There are four built-in operations, indicated by the first four natural numbers.

* `(0 n a b)`: Construct a new law: `{n a b}`
*   `(1 p l a n x)`: Pattern match on the value `x` (is it a pin, a law, an app, or a nat?).

    ```
    case x of
        PIN <i>     -> p i
        LAW {n,a,b} -> l n a b
        APP (f x)   -> a f x
        NAT x       -> n x
    ```
*   `(2 z p x)`: Pattern match on a natural number (is it zero or (n+1)?).

    ```
    case x of
        0 -> z
        x -> p (x-1)
    ```
* `(3 x)`: Increment a natural number.
* `(4 x)`: Normalize x and construct a new pin `<x>`.

More specifically:

* If `k` is 0:
  * It takes the second element of `e`, evaluates it using `F()`, and returns a new `'pin'` containing the evaluated value.
* If `k` is 1:
  * It extracts the name (`n`), arity (`a`), and body (`b`) from `e`.
  * It evaluates the name and arity using `N()` to convert them to natural numbers.
  * It returns a new `'law'` with the evaluated name, arity, and body (evaluated using `F()`).
* If `k` is 2:
  * It extracts the value `x` from `e`, evaluates it using `N()` to convert it to a `'nat'`, increments it by 1, and returns the result as a new `'nat'`.
* If `k` is 3:
  * It extracts the values `z`, `p`, and `x` from `e`.
  * It evaluates `x` using `N()` to convert it to a `'nat'`.
  * It calls the `C()` function with `z`, `p`, and the evaluated `x` to perform case matching on the `'nat'`.
* If `k` is 4:
  * It extracts the values `p`, `l`, `a`, `n`, and `x` from `e`.
  * It evaluates `x` using `E()` to reduce it to weak head normal form.
  * It calls the `P()` function with `p`, `l`, `a`, `n`, and the evaluated `x` to perform pattern matching on the PLAN value.

## The C() Function

```
C(z,p,n) = if n=0 then z else (p (n-1))
```

The `C()` function is responsible for pattern matching on natural numbers.

**Procedure:**

* If `n` is zero, it executes the action `z`.
* If `n` is greater than zero, it executes the action `p`, passing `n - 1` as the argument to `p`.
* If `n` does not match any of the specified patterns, the function does not perform any action.

## The P() Function

```
P(p,l,a,n,(f x))   = (a f x)
P(p,l,a,n,<x>)     = (p x)
P(p,l,a,n,{n a b}) = (l n a b)
P(p,l,a,n,x:@}     = (n x)
```

The `P()` function is responsible for pattern matching and executing the corresponding actions based on the type of the input expression.

**Inputs**:&#x20;

* `p`: The action to be executed if the input expression is a pin.
* `l`: The action to be executed if the input expression is a law.
* `a`: The action to be executed if the input expression is an app.
* `n`: The action to be executed if the input expression is a natural number.
* `o`: The input expression to be pattern matched.

**Procedure:**

* If `o` is an `'app'`, it applies the action `a` to the function and argument of the application.
* If `o` is a `'pin'`, it applies the action `p` to the value contained within the pin.
* If `o` is a `'law'`, it applies the action `l` to the name, arguments, and body of the law.
* If `o` is a '`nat'`, it applies the action `n` to the natural number value.
* If `o` does not match any of the specified types, the function does not perform any action.

## The F() Function

<pre><code>F(o) =
    E(o)
<strong>    when o:(f x)
</strong>         F(f); F(x)
    o
</code></pre>

The `F()` function is responsible for forcing a full evaluation of a value to its normal form (NF).&#x20;

**Procedure:**

* `F()` calls `E()`  on the input value `o` to evaluate it.
* If the type of `o` is an `'app'`, it recursively calls `F()` on the head and tail of the `'app'`.&#x20;

## The N() Function

```
N(o) = E(o); if o:@ then o else 0
```

The `N()` function is responsible for converting an expression into its corresponding natural number representation.

**Procedure:**

* The function first ensures that the expression `o` is in normal form by calling the `E()` function if necessary.
* If `o` is already a natural number, it simply returns `o`.
* If `o` is not a natural number, it converts it into a natural number representation using the following rules:
  * If `o` is a `'pin'`, it retrieves the value stored within the pin and converts it into a natural number representation.
  * If `o` is a `'law'`, it extracts the arguments of the law and converts them into a natural number representation.
  * If `o` is an `'app'` or any other type of expression, it recursively evaluates the expression to obtain its normal form.

## The I() Function

```
I(f, (e x), 0) = x
I(f, e,     0) = e
I(f, (e x), n) = I(f, e, n-1)
I(f, e,     n) = f

```

The function `I(f, o, n)` is an indexing function that retrieves an element from a list represented using nested `app`'s.

**Procedure:**

* The function recursively traverses the expression `o` based on the index `n`.
* If `n` is 0 and `o` is an application (type `'app'`), it returns the tail of the application.
* If `n` is 0 and `o` is not an application, it returns `o`.
* If `n` is non-zero and `o` is an application, it recurses into the head of the application, decrementing `n`.
* If `n` is non-zero and `o` is not an application, it returns `f`.

## The A() Function

```
A((f x))     = A(f)-1
A(<p>)       = A(p)
A({n a b})   = a
A(n:@)       = I(1, (3 5 3), n)
```

The `A(o)` function is designed to compute the arity (number of arguments) of a given expression `o`.

**Procedure:**

* If `o` is of type `'app'`, it recursively calls `A(o.head)` and subtracts 1 from the result. This means that for an `app` , the arity is determined by the arity of its `head` less 1.
* If the expression `o` is a pin (type `'pin'`), `A(o)` is recursively called on the value contained within the `'pin'`.
* If `o` is of type `law`, it returns the `nat` property of `o.args`, which represents the arity of the law.
* If `o` is of type `nat`, it returns 0, indicating that a number has an arity of 0.

## The R() Function

<pre><code>R(n,e,b:@) | b≤n = I(x,e,(n-b))
R(n,e,(0 f x))   = (R(n,e,f) R(n,e,x))
R(n,e,(1 v b))   = L(n,e,v,b)
R(n,e,(2 x))     = x
<strong>R(n,e,x)         = x
</strong></code></pre>

The function `R(n, e, b)` is responsible for running the body of a law (a user-defined function) using pattern matching.

Laws bodies are encoded using partially applied primops:

```
|---------|--------------------|
| pattern | meaning            |
|---------|--------------------|
| (0 f x) | (f x)              |
| (1 v b) | let v = b          |
| (2 k)   | const k            |
| 0       | self               |
| 1       | var 1              |
| 2       | var 2              |
| x       | const x (fallback) |
|---------|--------------------|
```

**Input Parameters**:

* `n`: The current normal form
* `e`: The environment
* `b`: The body expression being evaluated

**Procedure:**&#x20;

* The function first pattern matches on the structure of the expression `b`. It checks if the expression `b` matches any of the predefined patterns:
  * If `b` is a function application `(0 f x)`, it constructs a new application by recursively applying `R()` to both the function `f` and the argument `x`.
  * If `b` is a constant expression `(2 x)`, it returns the constant value `x`.
  * If `b` is a self-reference `(0 f x)`, it returns the function `f`.
  * If `b` is a let binding `(1 v b)`, it evaluates the let binding by updating the environment `e` with the value of the let-bound variable `v` and then proceeds to evaluate the body `b`.
  * If `b` is none of the above, it returns `b` unchanged.
* It continues this process recursively until it reaches a base case where no further pattern matching is possible.

## The L() Function

```
L(n,e,v,b) =
    x := <>
    f := (e x)
    x <- R(n+1,f,v)
    R(n+1,f,b)
    
The := operator creates a new local binding.
The <- operator overwrites an existing object in-place.
<> is a placeholder value for something that has not yet been evaluated.
```

The function `L()` is essentially a helper function used during the evaluation of let bindings within a law body. It is only called from within `R()`.

Laws bodies are encoded using partially applied primops:

```
|---------|--------------------|
| pattern | meaning            |
|---------|--------------------|
| (0 f x) | (f x)              |
| (1 v b) | let v = b          |
| (2 k)   | const k            |
| 0       | self               |
| 1       | var 1              |
| 2       | var 2              |
| x       | const x (fallback) |
|---------|--------------------|
```

**Procedure:**

* If the expression `x` is a let binding of the form `(1, v, b)`, where `1` represents a let binding, `v` is the variable being bound, and `b` is the body of the let binding:
  * It updates the value at index `n-i` in the environment `e` by calling `R(n, e, v)`, which evaluates the variable `v` in the current environment.
  * It recursively calls `L()` with `i+1` as the new index, `n` as the total number of arguments, `e` as the updated environment, and `b` as the new expression to be evaluated.
* If the expression `x` does not match the let binding pattern, it simply evaluates the expression `x` in the current environment by calling `R(n, e, x)`.
