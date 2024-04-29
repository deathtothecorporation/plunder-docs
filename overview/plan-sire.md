---
description: 'Document Type: Explanation'
---

# PLAN and Sire

PLAN is the evaluation model (you could think of this as the "machine code" of a Plunder VM) and Sire is a high-level functional language that compiles down to PLAN.  
We'll briefly explain both of them here.

{% hint style="warning" %}
**TODO: Brief description of PLAN, Sire**

This section needs work.

{% endhint %}

## PLAN

PLAN is an evaluation model that implements a self-contained, purely-functional database with no external dependencies.

{% hint style="warning" %}
TODO: Describe PLAN better.
**Let's actually rework some of the content from Jack's LambdaConf talk here.
{% endhint %}

The PLAN specification is quite concise:

```
Every PLAN vaue is either a pin x:<i>, a law x:{n a b}, an app x:(f g), a
nat x:@, or a black hole x:<>.  Black holes only exist during evaluation.

(o <- x) mutates o in place, replacing it's value with x.

Run F(x) to normalize a value.

E(o:@)     = o                          | F(o) =
E(o:<x>)   = o                          |     E(o)
E(o:(f x)) =                            |     when o:(f x)
    E(f)                                |          F(f); F(x)
    when A(f)=1                         |     o
        o <- X(o,o)                     |
        E(o)                            | N(o) = E(o); if o:@ then o else 0
    o                                   |
E(o:{n a b}) =                          | I(f, (e x), 0) = x
    if a!=0 then o else                 | I(f, e,     0) = e
        o <- <>                         | I(f, (e x), n) = I(f, e, n-1)
        o <- R(0,o,b)                   | I(f, e,     n) = f
        E(o)                            |
                                        | A((f x))     = A(f)-1
X((f x), e)         = X(f,e)            | A(<p>)       = A(p)
X(<p>, e)           = X(p,e)            | A({n a b})   = a
X({n a b}, e)       = R(a,e,b)          | A(n:@)       = I(1, (3 5 3), n)
X(0, (_ n a b))     = {N(n) N(a) F(b)}  |
X(1, (_ p l a n x)) = P(p,l,a,n,E(x))   | R(n,e,b:@) | b≤n = I(x,e,(n-b))
X(2, (_ z p x))     = C(z,p,N(x))       | R(n,e,(0 f x))   = (R(n,e,f) R(n,e,x))
X(3, (_ x))         = N(x)+1            | R(n,e,(1 v b))   = L(n,e,v,b)
X(4, (_ x))         = <F(x)>            | R(n,e,(2 x))     = x
                                        | R(n,e,x)         = x
C(z,p,n) = if n=0 then z else (p (n-1)) |
                                        | L(n,e,v,b) =
P(p,l,a,n,(f x))   = (a f x)            |     x := <>
P(p,l,a,n,<x>)     = (p x)              |     f := (e x)
P(p,l,a,n,{n a b}) = (l n a b)          |     x <- R(n+1,f,v)
P(p,l,a,n,x:@}     = (n x)              |     R(n+1,f,b)
```

## Sire

Sire is a lisp-like language whose purpose is to provide an ergonomic experience between a programmer's goals and the resulting PLAN that achieves these goals.

It also serves to allow the Plunder system to bootstrap itself, which avoids the need for opaque binaries.

{% hint style="warning" %}
TODO: Again here, let's use a modified version of Jack's Sire content from his talk. He's already done this explanation work, and very well.
{% endhint %}
