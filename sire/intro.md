# Intro to Sire

Syntax we'll need for the cog:

> Types
= top-level binding
| function application
* inlined function application (wut?)
@ let binding
?? named and pinned lambda
b#{} - Bar, byte array. padded (wut?)
: < col macro (the thing on the left is the args to a function)
# (as in # switch path) macro definition (wut? sire/switch.sire)
**SOME_CONST
() function application

& lambda (not really used in the demo cog)

Functions we'll need for the cog:

trk
fmapMaybe
hmLookup
barNat
natBar
barCat
getFiles (get* set* in general)
readRef
writeRef
newRef
syscall, fork
SOME (wut?)
map
weld
fromSome
tabLookup
tabFromPairs
barLen
hmInsert
hmSingleton
PIN

Data structures we'll need:

lists ~[], rows []
datacase, record

Other libraries we'll need to be familiar with:

json stuff (JMAP, JSTR, etc.)


main=() - cog running

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


