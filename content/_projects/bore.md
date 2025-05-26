---
name: bore
github: bore
description: A boring Makefile generator
layout: project
---

```
$ cat bore.lua
target {
	name = "main",
	default = true,
	build = rule {
		cmds = "echo 'Hello World!'"
	}
}

$ bore --make --make-file Makefile
$ cat Makefile
all: hello

hello:
	echo Hello World!

.PHONY: all hello

$ make
Hello World!

```

## About

Bore is a simple Makefile (or build.ninja) generator written in ANSI C. I was fed up with
bootstrapping Makefiles for simple projects, and I kept hitting friction trying to set up
out-of-source builds and other tasks just out of reach of an idiomatic Makefile. 

There are better tools for this now, but at the time I was proud of this little project.
