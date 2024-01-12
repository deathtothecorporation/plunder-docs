
# IO and `runCog`

```sire
(runCog someCog)
```

```sire
; Kernel State:
;     ( List Nat      ++  free slots
;     , Row Any       ++  slots row (vars or threads)
;     , Row Request   ++  requests row
;     )
```

Any given thread is tied to the requests row and the slots row.

**TODO: maybe not right:**
The state of the kernel is a structure of:
- the free slots
- the slots in-use
- outstanding requests
-

```sire
= (runCog act)

@ st,tid (allocateThread emptyState)
; emptyState is: (~[], [], [])
; (the ~[] in emptyState will evaluate to 0)

; allocateThread returns the State and ThreadId
; like this: [[0 [0] [0]] 0]
; so "@ st,tid (allocateThread emptyState)" results in:
;  - tid=0
;  - st=[0 [0] [0]]

; when allocating a thread in a non-empty state:

= someState (~[10], [], [])
;; looks like this: someState=[[10 0] [] []]
= st,tid (allocateThread someState)
;;;;;;
;   _g1005=[[0 [] []] 10]
;    st=[0 [] []]
;    tid=10

; another one:
= someOtherSTate (~[16 42], [], [])
;; looks like this: someOtherSTate=[[16 [42 0]] [] []]
= st,tid (alloc someOtherSTate)
;;;;;;;
;    _g1015=[[[42 0] [] []] 16]
;    st=[[42 0] [] []]
;    tid=16


@ st     (act nullThread tid st)
| ioLoop st (**getRequestsRow st)
```

allocateThread:

- if the free list is empty, (so evaluates to `0`)
- set `key` to `len slots`. in `emptyState` this is `[]`, or `0`
- set `slots` to concatenation of `slots` and `0`. in `emptyState` this is `[0]`
- set `requests` to concatenation of `requests` and `0`. in `emptyState` this is `[0]`
- set kernel state (`st`) to `[free slots requests]`. in `emptyState` this is
`[0 [0] [0]]` due to above steps.


---

the state of every cog is a function.
Let's say that you have state machine that takes in numbers, and spits out numbers.
type Machine = Integer -> (Integer, Machine)
We can make a machine that allows a number to be retrieved and modified by writing.

= (machine state input)
@ newState (intAdd input state)
| (newState, machine newState)

Then (machine 0) will retrieve the state.
And (machine 5) will increase by five (machine -5) will decrease by five, etc.
The way that the machine "stores the number", is by just returning a closure that contains that number.

If the number is 5, then the state of the machine is `(machine 5)`.  If you give that the input 5, then the new state of the machine will be `(machine 10)`.

in this ultra-trivialized state machine, the event log is just a list of numbers.  And a snapshot is just the actual PLAN value `(machine n)`.

Where the parens are indicating an APP node:
(machine 5) =
APP (PIN (LAW "machine" 2 ...)) (NAT 5)



