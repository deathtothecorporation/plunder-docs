# The Kernel, Briefly

{% hint style="warning" %}
**All below is TODO:**
{% endhint %}

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
