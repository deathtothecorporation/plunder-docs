# "Hello World" Cog

{% hint style="warning" %}
> **TODO:**
> - why does "boot" not trk on the second boot? where is this cached?!
> - why is it that the countdown runs on every "start" _only if_ the current
    time syscall is included? somewhere in the compilation/caching, something
    knows that a value is going to change on subsequent runs.
{% endhint %}

We're finally ready to see a simple cog. This file would be called
`sire/test_cog.sire`:

```test_cog.sire
#### test_cog <- prelude

/+  prelude

;;;;;

= (helloWorld return)
| trk "hello world"
| return ()

main=(runCog helloWorld)
```

# Running the Cog

Before we inspect the code, let's run it and see something happen!  

First, create the file `sire/test_cog.sire` with the above contents and save it.

Now, choose a directory where you want you plunder ships to get created. We'll use a
`my_ships` directory for the examples here. We'll proclaim that the name of the
ship that contains this cog will be "hello_world". Thus, the command to boot
this ship and initiate this cog is:

```
plunder boot ~/my_ships/hello_world sire/test_cog.sire
```

When you run this command, you'll see a whole bunch of output, with something
like this near the end:

```
<...>
("datom","LOADED FROM CACHE!")
("prelude","LOADED FROM CACHE!")
("test_cog","LOADED FROM CACHE!")
(helloWorld a)=(_Trace:{hello world} a-0)

{hello world}

main=(KERNEL [0 0] 0 [0] [0])

("cache hash","7y5a286DrMFXWwSJBiPAXYnpoYjqEfJcvSJuiBw1GKV4")
```

Congratulations, you've booted a plunder ship that runs a single cog which logs a string to the console.

# Understanding the Cog

Now let's go through it line-by-line to get a high-level understanding.

## Imports

We don't want to get too far into the weeds just yet with includes, boot order
and library imports, but we also don't want you to be puzzled by too many
unexplained lines of code.

```sire
#### test_cog <- prelude
```

This has to do with includes and load order. `test_cog.sire` is the name of the
current file, and `prelude.sire` is the name of a dependency. the `#### foo <-
bar` syntax says something like "Take `bar` and put it before `foo` and then
load `foo`".

```sire
/+  prelude
```

The `/+` rune imports a module. In this case, `prelude.sire` just does a bunch of
other importing of kernel code, convenience functions, syscalls, and basically
everything else we need to run a cog.



```
plunder start ~/my-ships/hello_world
```

```
<...>
[{2024-01-16T16:26:57.740950492Z} {REPLAY TIME: 0 ns}]

HTTP_SPINNING
_http_port=39023


= _http_port_file
} /home/your-user/my-ships/hello_world/11233545598979337509.http.port


Shutting down...
```


```sire
#### vc_countdown <- prelude

/+  prelude

;;;;;

= (countDownFrom count return)
: ??(syscall_time now) < syscall TIME_WHEN
| trk [%currentCount count %currentTime now]
| if (isZero count)
  | trk ["now we're done"]
  | return ()
| countDownFrom (dec count) return

main=(runCog | countDownFrom 5)
```


```
[{2024-01-16T16:28:11.604374602Z} {REPLAY TIME: 0 ns}]

HTTP_SPINNING
_http_port=35803


= _http_port_file
} /home/vcavallo/plunder/countdown/5977151470616073596.http.port



++ [%trk {2024-01-16T16:28:11.610577405Z}]
++ [%currentCount 5 %currentTime 1705422491]



++ [%trk {2024-01-16T16:28:11.610742661Z}]
++ [%currentCount 4 %currentTime 1705422491]



++ [%trk {2024-01-16T16:28:11.61086903Z}]
++ [%currentCount 3 %currentTime 1705422491]



++ [%trk {2024-01-16T16:28:11.611007429Z}]
++ [%currentCount 2 %currentTime 1705422491]



++ [%trk {2024-01-16T16:28:11.611120631Z}]
++ [%currentCount 1 %currentTime 1705422491]



Shutting down...
++ [%trk {2024-01-16T16:28:11.611265656Z}]
++ [%currentCount 0 %currentTime 1705422491]



++ [%trk {2024-01-16T16:28:11.611289355Z}]
++ [{now we're done}]

```
