# Test Drive: Syntactic Analysis

This solution defines a parser for a simple programming language with
assignment statements and if-else statements. The `endif` keyword makes
the grammar unambiguous. The plcc-ng tool is `plcc-parse`.

## Run it

```
cd 02-syntactic/solution
plcc-parse -s spec.plcc < testprog.txt
```

A successful parse prints `OK`.

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
(Ctrl-D) → `OK`

```
if a then a = 2
.
```
(Ctrl-D) → error (missing `endif`)

## Trace the parse

Add `-t` to see the parse tree being built step by step:

```
plcc-parse -s spec.plcc -t < testprog.txt
```

Ready for semantics? See `03-semantic/README.md`.
