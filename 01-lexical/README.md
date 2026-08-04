# Activity 1: Lexical Analysis

Lexical analysis (scanning) is the first phase of language processing. It
reads raw text and breaks it into **tokens** — named units like `INTEGER`,
`IF`, or `IDENT`. In plcc-ng, you describe tokens using Python regular
expressions in the lexical section of a `spec.plcc` file.

## Your task

Open `activity/spec.plcc`. It already defines:
- `WHITESPACE` (skipped)
- `COMMENT` (skipped)
- `IDENT`: identifiers like `foo`, `bar`, `x1`

Add three more tokens:
1. `INTEGER` — unsigned decimal integers (`0`, `42`, `1537`)
2. `IF` — the reserved word `if`
3. `ELSE` — the reserved word `else`

**Important:** Place `IF` and `ELSE` before `IDENT`. plcc-ng tries patterns
in order; if `IDENT` comes first, `if` would match as an identifier.

## Testing your scanner

Run plcc-scan interactively (type input, press Enter, see tokens, type
Ctrl-D when done):

```
plcc-scan -s activity/spec.plcc
```

Or scan a file:

```
plcc-scan -s activity/spec.plcc < activity/testprog.txt
```

`plcc-scan` prints each recognized token on its own line. Anything that
doesn't match a token is reported as an `error: unrecognized character`
diagnostic, and scanning continues with the next character.

## Check your work

Once your scanner recognizes `if`, `else`, and integers correctly, check your
results against the solution:

```
plcc-scan -s solution/spec.plcc < activity/testprog.txt
```

See `solution/README.md` for a walkthrough of the solution.
