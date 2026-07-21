# plcc-ng Demo

[plcc-ng](https://ourplcc.github.io/plcc-ng/latest/) is a tool for building
interpreters and compilers in the classroom. This demo lets you explore it
hands-on — no setup required. Click the button below to launch a ready-to-use
environment in GitHub Codespaces.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/ourPLCC/plcc-ng-demo)

---

## Test Drive (~15 minutes)

Want to see plcc-ng in action right away? Open a terminal in your Codespace
and navigate to the semantic solution:

```
cd 03-semantic/solution
```

Then follow the `README.md` there. You'll use all three plcc-ng tools on a
working prefix expression evaluator, and choose which programming language
to run the semantics in.

---

## Workshop (~60 minutes)

Want to experience what your students will? Work through the exercises in
order, starting here:

```
cd 01-lexical
```

Each directory has a `README.md` with instructions. You'll build a scanner,
then a parser, then a complete interpreter — one step at a time.

The three activities progress through the phases of language processing:

1. **`01-lexical/`** — define tokens with regular expressions
2. **`02-syntactic/`** — define grammar rules and build a parser
3. **`03-semantic/`** — add meaning: evaluate prefix arithmetic expressions

Solutions are in each `solution/` directory if you get stuck or want to
compare.
