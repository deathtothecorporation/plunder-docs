---
description: 'Document Type: Reference'
---

# Advanced Syntax To Recognize

{% hint style="warning" %}
  they'll see this in the cog but should not try to make too much sense of these parts.
{% endhint %}

They're briefly explained here only so that they can be recognized and maybe stupidly parroted if necessary, but not understood:

**All below are TODO:**

* `> Types`
* `**` inlined function application
* `??` named and pinned lambda
* `: (x y z) < foo x y | otherstuffLater x` - col macro
  * `foo x y` is the function call. the result of which is handled by the continuation (see below)
  * `(x y z)` is the continuation - what you want to do with result of `foo x y`.
* `##` (as in # switch path) macro definition (sire/switch.sire)

{% hint style="warning" %}
  Probably want to remove Kernel and IO (or move them to 'deeper'), in which case this comment below should change
{% endhint %}

We're almost ready to write our first Cog. If you're itching to just get something running, you could [skip ahead](/a-simple-cog/intro.md), but we suggest you take some time to familiarize yourself with the kernel and IO model:
