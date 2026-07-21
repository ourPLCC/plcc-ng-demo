# Test Drive: Syntactic Analysis

This solution defines a parser for a simple programming language with
assignment statements and if-else statements. The `endif` keyword makes
the grammar unambiguous. The plcc-ng tool is `plcc-parse`.

## Run it

```
cd 02-syntactic/solution
plcc-parse -s spec.plcc < testprog.txt
```

In plcc-ng, `plcc-parse` always prints the parse tree. You'll see the
tree structure for each statement in the program.

## Try it interactively

```
plcc-parse -s spec.plcc
```

Type a program and press Ctrl-D. Try valid and invalid programs:

```
a = 1
if a then a = 2 endif
.
```
(Ctrl-D) → parse tree output

```
if a then a = 2
.
```
(Ctrl-D) → error (missing `endif`)

Ready for semantics? See `03-semantic/README.md`.
