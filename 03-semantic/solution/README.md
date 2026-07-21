# Test Drive: Prefix Expression Evaluator

This is the recommended starting point for the Test Drive path. This solution
implements a prefix arithmetic interpreter using all three phases of language
processing — and you can run the semantic interpreter in Python, Java, or
JavaScript.

Navigate to this directory first:

```
cd 03-semantic/solution
```

---

## Step 1: Scan — tokens

`plcc-scan` tokenizes input into named tokens. Run it on the Python solution:

```
plcc-scan -s python/spec.plcc < python/testprog.txt
```

Each token is printed as `source:line:col TOKEN 'lexeme'` (where `-` means stdin):

```
-:1:1 INTEGER '3'
-:2:1 ADDOP '+'
-:2:3 INTEGER '3'
-:2:5 INTEGER '9'
...
```

Try scanning interactively — type an expression and press Enter:

```
plcc-scan -s python/spec.plcc
```

(Press Ctrl-D on a new line to exit.)

---

## Step 2: Parse — structure

`plcc-parse` checks that the token stream matches the grammar and prints the
parse tree if it succeeds.

```
plcc-parse -s python/spec.plcc < python/testprog.txt
```

Parse interactively:

```
plcc-parse -s python/spec.plcc
```

---

## Step 3: Evaluate — pick your language

`plcc-rep` is the read-eval-print loop. It scans, parses, and evaluates each
expression, printing the result.

**Python:**
```
plcc-rep -s python/spec.plcc < python/testprog.txt
```

**Java:**
```
plcc-rep -s java/spec.plcc < java/testprog.txt
```

**JavaScript:**
```
plcc-rep -s javascript/spec.plcc < javascript/testprog.txt
```

All three should produce the same output:
```
3
12
1
-1
-3
90
6
90
-90
-63
63
```

Run interactively to try your own expressions:

```
plcc-rep -s python/spec.plcc
```

Try:
```
+ 10 5
* + 2 3 4
_ * 3 7
```
(Ctrl-D to exit)

---

## What just happened?

The `spec.plcc` file has three sections separated by `%`:

1. **Lexical** — regular expressions define tokens
2. **Syntactic** — BNF grammar rules describe structure
3. **Semantic** — Python/Java/JavaScript methods give meaning

plcc-ng reads the spec directly — no separate build or compile step.

---

Want to build this yourself? See `../README.md` for the workshop activity.
