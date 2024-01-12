# TODO: stuff they'll see but should ignore

they'll see this in the cog but should not try to make too much sense of these
parts.

They're briefly explained here only so that they can be recognized and maybe
stupidly parroted if necessary, but not understood:

> Types
>
* inlined function application
*
?? named and pinned lambda

b#{} - Bar, byte array. padded

`: (x y z) < foo x y  | otherstufflater x`  - col macro (the thing on the left is the args to a function)
  - `foo x y` is the function call. the result of which is handled by the
  continuation (see below)
  - `(x y z)` is the continuation - what you want to do with result of `foo x
  y`.

# (as in # switch path) macro definition (sire/switch.sire)

**SOME_CONST
