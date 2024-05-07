---
description: 'Document Type: Explanation'
---
# UI files

{% hint style="warning" %}
TODO: Explanation of UI file handling is forthcoming. To include:

-  Use mandelbrot UI as an example (since it's already been seen)
-  Explain the `=(fileServer ...)` binding. it responds to GET requests and
responds with an HTTP response for a file (index.html, index.js, etc)
- Explain `=(handleReq ...)` as it relates to handling the initial PUT requests containing UI files
  -  show a snippet from the .sh files where the UI files are thrown into the cog after booting (then subsequently retrieved when the browser GETs)

This implementation of UI is very likely to change in the future. Consider this
a proof of concept that is nonetheless quite useful in the interim and wise to
learn regardless.
{% endhint %}

