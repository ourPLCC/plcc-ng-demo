# Activity 2: Syntactic Analysis

Syntactic analysis (parsing) is the second phase of language processing. It
checks whether a sequence of tokens follows the grammar rules of your
language, and builds a parse tree representing the structure.

In plcc-ng, you describe grammar rules using LL(1) BNF notation in the
syntactic section of a `spec.plcc` file.

## The problem: ambiguity

Open `activity/spec.plcc` and look at the grammar for if-statements:

```
<Stmt:IfStmt>     ::= IF <Expr:expr> THEN <Stmt:stmt> <ElsePart:elsePart>
<ElsePart:EPnada> ::=
<ElsePart:EPelse> ::= ELSE <Stmt:stmt>
```

This grammar is **ambiguous**. An LL(1) parser reads one token ahead to
decide which rule to apply. When it sees an `IF` statement with a nested
`IF` inside, it can't tell whether an `ELSE` belongs to the inner `IF`
or the outer `IF`.

## Your task

Fix the ambiguity by requiring every `if` statement to end with a new
keyword `endif`. This makes the nesting structure explicit and unambiguous.

1. Add an `ENDIF` token to the lexical section (place it before `IDENT`)
2. Update the `IfStmt` grammar rule to require `ENDIF` at the end

## Testing your parser

Try parsing the test program:

> **Note:** The test program already uses `endif`. The starting spec will fail on it — that's expected. Fix the spec first, then run the test.

```
plcc-parse -s activity/spec.plcc < activity/testprog.txt
```

In plcc-ng, `plcc-parse` always prints the parse tree. A successful parse
shows the tree structure; an ambiguous or incorrect grammar produces an error.

Parse interactively (type one program at a time, press Ctrl-D to finish):

```
plcc-parse -s activity/spec.plcc
```

## Check your work

```
plcc-parse -s solution/spec.plcc < activity/testprog.txt
```

See `solution/README.md` for a walkthrough.
