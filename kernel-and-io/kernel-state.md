# The Kernel, Briefly

{% hint style="warning" %}
TODO. Don't read this yet.
{% endhint %}

```sire
; Kernel State:
;     ( List Nat      ++  free slots
;     , Row Any       ++  slots row (vars or threads)
;     , Row Request   ++  requests row
;     )
```

Any given thread is tied to the requests row and the slots row.
