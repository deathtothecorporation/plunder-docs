# Intro

TODO:

- From repl to cog. the differences in writing Sire files vs testing in repl
- identify "boilerplate", but briefly explain what it does
- identify areas for user to customize


The mechanic that's more interesting here w.r.t how the cog state works is `modifyState`.
Which is using `readRef` and `writeRef` to modify "slots" in the `kern.sire` state:

> Ref CogState > (CogState > CogState) > Cog ()
= (modifyState vSt fun return)
: (PIN old) < readRef vSt
@ srv       | **getServThread old
@ pNew      | PIN (fun old)
: _         < writeRef vSt pNew
: _         < cancelFork srv (syscall (**HTTP_SERV | fileServer pNew))
| return ()


