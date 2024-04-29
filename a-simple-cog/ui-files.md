---
description: 'Document Type: Tutorial'
---
# TODO: UI files

{% hint style="info" %}
This implementation of UI is very likely to change in the future. Consider this
a proof of concept that is nonetheless quite useful in the interim and wise to
learn regardless
{% endhint %}


{% hint style="warning" %}
**TODO:** Fill out this explanation

- [ ] Use mandelbrot UI as an example (since they've seen it already earlier on)
- [ ] Reminder about "state" from [cogs.md](kernel-and-io/cogs.md)
- [ ] Explain the `=(fileServer ...)` binding. it responds to GET requests and
responds with normal HTTP response for a file (index.html, index.js, etc)
- [ ] Explain `=(handleReq ...)` as it relates to handling the initial PUT
    requests
  - [ ] show a snippet from the .sh files where the UI files are thrown into the
      cog after booting (then subsequently retrieved when the browser GETs)

TODO: finally, explain this pattern:

```sire
> Ref CogState > Cog Void
= (runHttpServer vSt return)
: ??(rhs_heard req) < syscall HTTP_HEAR
: _                 < handleReq vSt req
| runHttpServer vSt return

> Cog ()
= (launchDemo return)
: servThread  < fork (syscall (**HTTP_SERV emptyFileServer))
: vSt         < newRef (PIN | newState servThread)
: _           < modifyState vSt id
: httpThread1 < fork (runHttpServer vSt)
: httpThread2 < fork (runHttpServer vSt)
| return ()

> PausedCog
main=(runCog launchDemo)
```

TODO: That is, how do the threads interact with [kernel state](kernel-and-io/kernel-state.md), and how is it that this
cog keeps running? How do HTTP requests get heard and answered by this cog? why
two http threads?
{% endhint %}

---

Having an interface is great. Having an interface that allows us to make persisted state changes on the backend is the stuff that civilizations are built on. Read on:
