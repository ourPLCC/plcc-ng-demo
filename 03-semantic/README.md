# Activity 3: Semantic Analysis

Semantic analysis gives meaning to your language's syntax. In plcc-ng,
you add a semantic section to your spec that defines Python (or Java or
JavaScript) methods on each parse tree node class.

This activity builds a **prefix expression evaluator**. In prefix notation,
the operator comes before its operands:

```
+ 3 9      → 12
- 10 4     → 6
* + 2 3 4  → 20   (i.e., (2 + 3) * 4)
```

## How it works

plcc-ng generates a class for each grammar rule. Each class gets fields
matching the named items on the right-hand side of its rule. You define
methods on those classes in the semantic section.

For example, the rule:

```
<Expr:Add> ::= ADDOP <Expr:opnd1> <Expr:opnd2>
```

generates an `Add` class with fields `opnd1` and `opnd2` (both `Expr`
instances). In the semantic section:

```python
Add
%%%
def evaluate(self):
    return self.opnd1.evaluate() + self.opnd2.evaluate()
%%%
```

## Your task

Open `activity/spec.plcc`. The `Add` class is implemented for you. Complete:

1. `Sub`, `Mul`, and `Div` — follow the `Add` pattern. This interpreter is
   integer-only (see `Const.evaluate`, which parses with `int(...)`), so
   `Div` should use integer/floor division (`//` in Python) rather than
   true division (`/`) — otherwise results come out as floats (`-63.0`
   instead of `-63`).
2. Unary negation: a `_` operator that negates a single operand
   - Add a `UMIOP` token for `_` in the lexical section
   - Add a grammar rule `<Expr:Umi> ::= UMIOP <Expr:opnd1>`
   - Add a `Umi` class in the semantic section

## Testing your interpreter

Run with a file:

```
plcc-rep -s activity/spec.plcc < activity/testprog1.txt
```

Or interactively (one expression at a time, Ctrl-D to quit):

```
plcc-rep -s activity/spec.plcc
```

`plcc-rep` is a read-eval-print loop — it evaluates each expression and
prints the result.

## Check your work

Compare against the solution. Solutions are also available in Java and
JavaScript — see `solution/README.md`.

```
plcc-rep -s solution/python/spec.plcc < activity/testprog2.txt
```
