# Test Drive: Lexical Analysis

This solution defines a scanner for a simple programming language with
identifiers, integers, and reserved words. The plcc-ng tool is `plcc-scan`.

## Run it

From the repo root or from `01-lexical/solution/`, run:

```
plcc-scan -s 01-lexical/solution/spec.plcc < 01-lexical/solution/testprog.txt
```

Or navigate into the directory first:

```
cd 01-lexical/solution
plcc-scan -s spec.plcc < testprog.txt
```

You should see output like (excerpt — actual output contains earlier tokens first):
```
IDENT(Hi_there)
...
IF(if)
ELSE(else)
IDENT(biff)
INTEGER(0)
...
ERROR(49.68)
```

## Try it interactively

Run without a file to enter input from the keyboard:

```
plcc-scan -s spec.plcc
```

Type tokens and press Enter. Press Ctrl-D on a new line to finish.

## What's in the spec

The scanner matches tokens in order. `IF` and `ELSE` appear before `IDENT`
so reserved words take priority — `if` matches `IF`, not `IDENT`.

Ready for the next step? See `02-syntactic/README.md`.
