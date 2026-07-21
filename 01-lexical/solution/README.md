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

Each token is printed as `source:line:col TOKEN 'lexeme'` (where `-` means stdin).
You should see output like:
```
-:2:1 IDENT '%'
-:3:1 IDENT '$%'
-:4:1 IDENT '@@%$%Hi_there'
...
-:10:1 IF 'if'
-:11:1 ELSE 'else'
-:12:1 IDENT 'biff'
...
-:18:1 INTEGER '49'
-:18:3 ERROR '.'
-:18:4 INTEGER '68'
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
