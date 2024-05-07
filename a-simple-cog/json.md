---
description: 'Document Type: Explanation'
---

# JSON

## Handling JSON requests

We're going to look at some selections from the `sire/demo_mandelbrot_ui.sire` cog that you saw when you first installed Plunder on your system.

```sire
:| json

;; <etc> ;;


= (asJsonRow m)
# datacase m
* JVEC|v | SOME v
* _      | NONE

= (asJsonNum m)
# datacase m
* JNUM|n | SOME n
* _      | NONE

(bindMaybe mVal k)=(maybeCase mVal NONE k)

;; <etc> ;;

= (parseFract jsonBS)
@ res@[json leftover] (parseJson jsonBS)
: rw < **bindMaybe (asJsonRow json)
: w < **bindMaybe (asJsonNum idx-0-rw)
: h < **bindMaybe (asJsonNum idx-1-rw)
| SOME [w h]

;; <etc> ;;
# switch method
* POST
  # switch path
  * b#{/fract}
    # datacase (parseFract body)
    * NONE
      : _ < fork (syscall (**HTTP_ECHO rid 400 b#bad [] b#{}))
      | return ()
    * (SOME dims)
      @ [w h] dims
      : !fractBar < trkTimer {mandelbrotDemo} (mandelbrotDemo w h)
      : _ < fork (syscall (**HTTP_ECHO rid 200 b#ok [] fractBar))
      | return ()
;; <etc> ;;
```
Taking that one chunk at a time:

```
:| json
```

Import `json.sire`, from which we'll need the `parseJson` function soon.

```
= (asJsonRow m)
# datacase m
* JVEC|v | SOME v
* _      | NONE

= (asJsonNum m)
# datacase m
* JNUM|n | SOME n
* _      | NONE

(bindMaybe mVal k)=(maybeCase mVal NONE k)
```

We'll look at `datacase` next, but consider it a sort of switch or if/else for now.

`asJsonRow` takes some parsed JSON and returns it as a sire row (if there's anything there; that's where `datacase` comes in).  
`asJsonNum` takes some parsed JSON and returns it as a nat.  
`sire/json.sire` has similar helpers for JSON's nulls, boolens, strings and maps (objects).

```
= (parseFract jsonBS)
@ res@[json leftover] (parseJson jsonBS)
: rw < **bindMaybe (asJsonRow json)
: w < **bindMaybe (asJsonNum idx-0-rw)
: h < **bindMaybe (asJsonNum idx-1-rw)
| SOME [w h]
```

The mandelbrot backend accepts a `width` and `height` from the client side and generates a fractal of these dimensions.

`parseFract` takes a JSON-y bar (which it happens to expect to be a single "row" of json numbers) and uses the above-described two helpers to bind these values to `w` and `h`. Width and height.

Now that we have all the handlers in place, we can actually take a request from the browser:

```
# switch method
* POST
  # switch path
  * b#{/fract}
    # datacase (parseFract body)
    * NONE
      : _ < fork (syscall (**HTTP_ECHO rid 400 b#bad [] b#{}))
      | return ()
    * (SOME dims)
      @ [w h] dims
      : !fractBar < trkTimer {mandelbrotDemo} (mandelbrotDemo w h)
      : _ < fork (syscall (**HTTP_ECHO rid 200 b#ok [] fractBar))
      | return ()
```

We haven't yet looked at how a webserver cog works, but the point here is:
- We get a `POST` request at a particular path from the web client
- We parse the request body as JSON
- Assuming we got dimensions as expected, we generate the fractal (`fractBar`) and respond to the request with this value.

## Responding **with** JSON


---

Once we're Creating, Reading, Updating and Deleting data on the backend, it will be helpful to have some predictable data structures to work with. Continue:
