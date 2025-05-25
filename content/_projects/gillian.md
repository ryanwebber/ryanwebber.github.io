---
name: gillian
github: gillian
description: A stack-based code golf language
layout: project
---

```
# Print the first 100 fibinacci numbers
$ echo 'p1pC{2P+' | gillian

0
1
1
2
3
5
8
13
21
34
55
89
144
233
377
610
987
1597
...
```

## About

Code golf is a type of programming where the goal is to solve a problem using the
smallest amount of source code possible. Many people have created their own
programming languages for this specific purpose -- an elite, prestigious club that
I am now a proud member of.

My entry is called gillian. It's a stack-based language, with various inspirations.
Each character is a command that operates on that stack. With some clever tricks and
a bit of puzzle-solving, you can create surprisingly concise programs.

Let's take a look at the above example `p1pC{2P+` to see how this works, character by character:
 - `p` - duplicates the top value of the stack. When the stack is empty (which is the case
   here), a `0` is pushed. The stack is now `[0]`.
 - `1` - a set of consecutive digits forms a number, which is pushed to the stack. The
   stack is now `[0, 1]`.
 - `p` - duplicates the top value of the stack. The stack is now `[0, 1, 1]`.
 - `C` - multiply the top value on the stack by 100. The stack now contains `[0, 1, 100]`. Using
   the `p` and `C` allowed us to push the number 100 with only two characters -- the kind
   of puzzle-solving thinking that is used to code golf.
 - `{` - pop a value off the stack. Run the following block (implicitly terminated by EOF) that
   many times (100 in this case). The stack is now `[0, 1]`.
   - `2` - push the number 2 onto the stack. The stack is now `[0, 1, 2]`.
   - `P` - pop a value off the stack. Duplicate that many values on top of the stack. The stack now contains `[0, 1, 0, 1]`.
   - `+` - pop the top 2 values off the stack and push the sum back to the stack. The stack now contains `[0, 1, 1]`.
   - ... the block repeats, copying the top 2 stack values and replacing them with their sum.

When the program terminates, the values on the stack are printed, one per line, giving us our Fibonacci sequence!

The fun of this project is just in the design of the language -- determinging the syntax,
coming up with valuable commands, trying to reduce duplicate ways of doing the same thing,
and making it feel rewarding to solve problems with it. 
