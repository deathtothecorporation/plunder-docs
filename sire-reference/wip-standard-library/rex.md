---
hidden: true
---

# Rex

### bopE

Creates a Rex node representing a function application in open mode.

```sire
bopE [varE 'f' varE 'x' varE 'y']  ; Returns a Rex node for (f x y)
bopE [varE '+' natE 1 natE 2]      ; Returns a Rex node for (+ 1 2)
bopE [varE 'single']               ; Returns a Rex node for single
```

### bapE

Creates a Rex node representing a function application in open mode with the last argument as heir.

```sire
bapE [varE 'f' varE 'x' varE 'y']  ; Returns a Rex node for (f x
                                   ;                           y)
bapE [varE 'if' varE 'cond' varE 'then' varE 'else']
                                   ; Returns a Rex node for (if cond then
                                   ;                            else)
bapE [varE 'single']               ; Returns a Rex node for single
```

### bowE

Creates a Rex node representing a row in open mode.

```sire
bowE [natE 1 natE 2 natE 3]  ; Returns a Rex node for (| 1 2 3)
bowE [varE 'a' varE 'b']     ; Returns a Rex node for (| a b)
bowE []                      ; Returns a Rex node for (|)
```

### appE

Creates a Rex node representing a function application.

```sire
appE [varE 'f' varE 'x' varE 'y']  ; Returns a Rex node for (f x y)
appE [varE '+' natE 1 natE 2]      ; Returns a Rex node for (+ 1 2)
appE [varE 'single']               ; Returns a Rex node for single
```

### rowE

Creates a Rex node representing a row.

```sire
rowE [natE 1 natE 2 natE 3]  ; Returns a Rex node for [1 2 3]
rowE [varE 'a' varE 'b']     ; Returns a Rex node for [a b]
rowE []                      ; Returns a Rex node for []
```

