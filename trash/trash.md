
# Delete:

Of course, we can nest functions and now is a good a time as any to see this in
action:

```sire
(sub 10 1)
9
; sub, like add, does basic addition. subtracting the second arg from the first

(len arr)
3

(sub (len arr) 1)
     ;   3
2

(idx (sub (len arr) 1) arr)
42
; we got index 2 in the row, which was 42, the last entry.
```

Thankfully, we don't have to: `(last arr) == 42`

