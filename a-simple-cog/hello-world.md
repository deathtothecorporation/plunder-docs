# Lets See a Cog Already!

> **TODO:**
> - why does "boot" not trk on the second boot? where is this cached?!
> - why is it that the countdown runs on every "start" _only if_ the current
    time syscall is included? somewhere in the compilation/caching, something
    knows that a value is going to change on subsequent runs.


```test_cog.sire
#### test_cog <- prelude

/+  prelude

;;;;;

= (helloWorld return)
| trk ["hello world"]
| return ()

main=(runCog helloWorld)
```

```
plunder boot ~/my-ships/hello_world sire/test_cog.sire
```

```
<...>
("datom","LOADED FROM CACHE!")
("prelude","LOADED FROM CACHE!")
("test_cog","LOADED FROM CACHE!")
(helloWorld a)=(_Trace:[{hello world}] a-0)

[{hello world}]

main=(KERNEL [0 0] 0 [0] [0])

("cache hash","7y5a286DrMFXWwSJBiPAXYnpoYjqEfJcvSJuiBw1GKV4")
```

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
