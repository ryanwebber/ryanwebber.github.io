---
name: rum
github: rum
description: A Lua-flavoured lisp
layout: project
---

```
; Functional
(map [1 2 3] (lambda! (x) (+ x 1)))

; Meta-programming
(set (#meta :locals) "myvar" 42)
(+ myvar 1)
```

## About

I really like the idea of lisps, but unlike other functional languages, I've
never built anything with them. So instead of building something with a lisp,
I decided to build a lisp. Specifically, I wanted an _embedded lisp_, because
I like the idea of using a lisp as a scripting language for future game
development projects, and because there's only a couple of embeddable options
out there. I also thought it might be interesting to try to expose interpreter
state via meta-programming facilities similar to Lua.

The rest of the story is pretty standard: there's not much complexity to the
parser, and the tree-walking interpreter is straightforward. Thanks to the 
interpreter state being exposed by the meta-programming facilities, the interpreter
doesn't actually implement any primitives of its own -- instead, core modules
like `def-macro!` are loaded at runtime, which theoretically allows
for entirely different front-ends and semantics.

While basic functionality is implemented, many standard library modules are
missing which would make the language actually useful. Rather than spending
my time implementing this, I've actually been using [Fennel](https://fennel-lang.org)
for my Lua-flavoured lisp needs.

Like all my other experiments, I'll probably come back to this at some point.
