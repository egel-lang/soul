# Soul - an esoteric concatenative language

Soul is an esoteric concatenative one-stack language, a 
small explorative language with an interpreter written in
a day.

Most concatenative languages work on a state, often that's
another stack. For example, Forth has a data and return stack
and Postscript has an extra stack outputting to a page.

Soul is a minimalist language that provides one thing, and
one thing only, the one sole stack. That, of course, is
very problematic as a model of computation but Soul gets
around that with two tricks:

1. Treat the stack as random access memory
2. Whenever a constant is found on top of the stack, that
   gets swapped with whatever is underneath.

Soul expressions consist of integers, texts, and words. Words
may define other expressions. A Soul program runs until the 
stack is empty.

## Predefined Operators

`fetch`, `put`, and `delete` inspect or modify the stack, they all
take an integer argument where `put` takes a second argument to put somewhere
in the stack.

The arithmetic operators `+`, `-`, `*`, `/` are all binary prefix
operators. `+` is overloaded to work on texts also.

The equality operator `=` tests two arguments for equivalence, it
results in either `true` or `false`. Both booleans are functions
that select between two arguments; e.g., `true f g` evaluates to
`f`.

`print` and `line` are the basic input/output words, `to_int` and
`to_text` convert between integers and texts.

## Examples

Hello World example:

```
"hello" print "world" print
```

Output 42, showcasing the defintion of a word:

```
# calculate the answer to everything
:f  * 6
+ 3 4 f print
```

The faculty function:

```
# the faculty function

# get
:get2   delete 1
:get1   fetch 5 + 3 delete get2
:get0   fetch 4 + 4 fetch get2 
:get    fetch 2 get0 get1 

# fac
:fac3   get 2 fac *
:fac2   fetch 3 - 1 fac3
:fac1   delete 1 1
:fac0   get 4 = 0 fac1 fac2
:fac    fetch 1 fac0 

fac 6 print
```

The Soul interpreter is written as an Egel script and
implements a REPL. 

You can run the interpreter with `egel soul.eg`. Either
pipe the example programs to that process or copy-paste
the source directly into the REPL.
