# Dependencies and Installation

**TODO: Briefly explain nix, then assume its being used** (outside the scope of
this document to fully cover all environments).

Any time you open a new terminal, you must remember to enter the development
shell with nix by running:

```bash
nix develop
```

You will forget to do this and wonder why nothing is working. That's why.

Once you're in a nix development shell, run the following command to build the
plunder environment:

```bash
stack build
```

This might take a while! When it's done, you should be able to verify all went
well by running the mandelbrot web app demo.

```bash
bash sh/mandelbrot-ui-demo
```

That command should output something like this:


```shell

<...A lot more stuff up here...>

tabSplitLT          MATCHED
    , e "tabSplitLT"          "UdzPypcfJLtyMjLy87ukmU7nRHtfEv3nnZbfJ34isWM"
tabSing             MATCHED
    , e "tabSing"             "7AfouhmiExP9KSySjrw4k4VZ5JY2TE4cW53xo1EhHsfz"
[{2024-01-12T16:22:20.897758578Z} {REPLAY TIME: 0 ns}]

HTTP_SPINNING
_http_port=56191


= _http_port_file
} /home/your-user/plunder/.demo/4172716296342791002.http.port


Setting interface files...
Interface files are set. Running on http://localhost:56191/index.html
```

There is now a webserver running and you can access the app at the URL mentioned in
the final line of output. **NOTE: the `index.html` at the end is required!**



