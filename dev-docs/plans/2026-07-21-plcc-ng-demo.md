# plcc-ng-demo Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a self-directed demo repository at `/Users/stoney/Workspace/plcc-ng-demo` that lets instructors evaluate plcc-ng by launching a GitHub Codespace and exploring realistic workshop exercises updated for plcc-ng syntax.

**Architecture:** Directory-driven structure with numbered topic directories (`01-lexical/`, `02-syntactic/`, `03-semantic/`), each containing an `activity/` for hands-on work and a `solution/` with a test-drive README. The root README offers two paths: Test Drive (~15 min, start at `03-semantic/solution/`) and Workshop (~60 min, work through all three topics in order). `03-semantic/solution/` has language sub-directories (`python/`, `java/`, `javascript/`) to showcase plcc-ng's multi-language semantic support.

**Tech Stack:** plcc-ng (via `ghcr.io/ourplcc/devcontainers/plcc-ng:1` devcontainer image), GitHub Codespaces

## Global Constraints

- All spec files named `spec.plcc` (not `grammar` or `idents.plcc` etc.)
- plcc-ng syntax throughout: `%` section separators, `%%%` code block delimiters, `Python`/`Java`/`JavaScript` language headers (no `%` prefix on language header)
- Nonterminals in PascalCase: `<Prog>`, `<Expr>`, `<Stmt>`, `<ElsePart>`, `<Stmts>`
- LHS subclass notation: `<Nonterminal:ClassName>` e.g. `<Expr:Add>`
- RHS field notation: `<Nonterminal:fieldName>` e.g. `<Expr:opnd1>` or `<INTEGER:integer>`
- Token RHS without field: bare UPPERCASE e.g. `ADDOP`, `IF`, `ENDIF`
- Token RHS with field: `<UPPERCASE:fieldName>` e.g. `<INTEGER:integer>`, `<IDENT:ident>`
- Entry point method: `_run()` (not `$run()`)
- Semantic code blocks: `ClassName\n%%%\n...code...\n%%%` (not `%%{...%%}`)
- Python field access: `self.fieldName.lexeme` for tokens, `self.fieldName.methodName()` for nonterminals
- Java field access: `fieldName.toString()` for tokens, `fieldName.methodName()` for nonterminals
- No build step needed: plcc-ng tools (`plcc-scan`, `plcc-parse`, `plcc-rep`) run directly on the spec file
- Interaction examples must be honest: interactive stdin or file redirect, never `echo | pipe`
- Issues filed in `dev-docs/issues/` with a **Target repo:** note indicating where they belong upstream
- plcc-ng is NOT installed locally — verification must be done in GitHub Codespaces

## Source Materials

- CCSCNE-2026 repo cloned to scratchpad: `/private/tmp/claude-502/-Users-stoney-Workspace/7521eb23-c84e-43b9-a84c-241d2f4c3778/scratchpad/CCSCNE-2026/`
- Issue infrastructure to copy from: `/Users/stoney/Workspace/plcc-ng/`
- Key source files:
  - `Activity1/idents.plcc` + `Sol/Activity1-sol/idents.plcc` → `01-lexical/`
  - `Activity2/if-stmt.plcc` + `Sol/Activity2-sol/if-stmt.plcc` → `02-syntactic/`
  - `Activity3/prefix.plcc` + `Sol/Activity3-sol/prefix.plcc` → `03-semantic/`

---

### Task 1: Scaffold repo and infrastructure

**Files:**
- Create: `/Users/stoney/Workspace/plcc-ng-demo/` (entire repo)
- Create: `.devcontainer/devcontainer.json`
- Create: `.gitignore`
- Copy: `dev-docs/issues/TEMPLATE.md`, `dev-docs/issues/.next-id.txt`, `dev-docs/issues/done/`
- Copy: `bin/issues/new.bash`, `bin/issues/close.bash`, `bin/issues/check.bash`
- Create: `dev-docs/plans/`, `dev-docs/specs/`
- Create: `dev-docs/specs/2026-07-21-plcc-ng-demo-design.md`

- [ ] **Step 1: Create repo directory and git init**

```bash
mkdir -p /Users/stoney/Workspace/plcc-ng-demo
cd /Users/stoney/Workspace/plcc-ng-demo
git init
```

- [ ] **Step 2: Create .devcontainer/devcontainer.json**

```bash
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/.devcontainer
```

File `/Users/stoney/Workspace/plcc-ng-demo/.devcontainer/devcontainer.json`:
```json
{
  "name": "plcc-ng Demo",
  "image": "ghcr.io/ourplcc/devcontainers/plcc-ng:1"
}
```

- [ ] **Step 3: Create .gitignore**

File `/Users/stoney/Workspace/plcc-ng-demo/.gitignore`:
```
__pycache__/
*.pyc
.DS_Store
```

- [ ] **Step 4: Copy issue tracking infrastructure from plcc-ng**

```bash
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/dev-docs/issues/done
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/bin/issues

cp /Users/stoney/Workspace/plcc-ng/dev-docs/issues/TEMPLATE.md \
   /Users/stoney/Workspace/plcc-ng-demo/dev-docs/issues/TEMPLATE.md

echo "001" > /Users/stoney/Workspace/plcc-ng-demo/dev-docs/issues/.next-id.txt

cp /Users/stoney/Workspace/plcc-ng/bin/issues/new.bash \
   /Users/stoney/Workspace/plcc-ng-demo/bin/issues/new.bash
cp /Users/stoney/Workspace/plcc-ng/bin/issues/close.bash \
   /Users/stoney/Workspace/plcc-ng-demo/bin/issues/close.bash
cp /Users/stoney/Workspace/plcc-ng/bin/issues/check.bash \
   /Users/stoney/Workspace/plcc-ng-demo/bin/issues/check.bash
chmod +x /Users/stoney/Workspace/plcc-ng-demo/bin/issues/*.bash
```

- [ ] **Step 5: Create remaining dev-docs directories**

```bash
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/dev-docs/plans
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/dev-docs/specs
```

Create `/Users/stoney/Workspace/plcc-ng-demo/dev-docs/roadmap.md`:
```markdown
# plcc-ng-demo Roadmap

## Open Issues

(none yet)
```

- [ ] **Step 6: Write design spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/dev-docs/specs/2026-07-21-plcc-ng-demo-design.md`:
```markdown
# plcc-ng-demo — Design

**Date:** 2026-07-21
**Status:** Approved

---

## 1. Goal

Create a self-directed demo repository for plcc-ng targeting instructors
evaluating the tool for classroom use. Participants launch via GitHub
Codespaces and explore realistic workshop exercises — no setup, no
facilitator required.

---

## 2. Audience

Instructors considering using plcc-ng in a course. They want to see how easy
it is to get started, what a lesson or assignment looks like, and what
students will experience.

---

## 3. Structure

Directory-driven, numbered by topic:

```
plcc-ng-demo/
├── .devcontainer/devcontainer.json
├── README.md
├── 01-lexical/      (activity/ + solution/)
├── 02-syntactic/    (activity/ + solution/)
└── 03-semantic/     (activity/ + solution/{python,java,javascript}/)
```

---

## 4. Two Paths

**Test Drive (~15 min):** Navigate to `03-semantic/solution/`, pick a
language, follow that README. Demonstrates all three plcc-ng tools.

**Workshop (~60 min):** Work through 01 → 02 → 03 in order, completing
activities and checking solutions.

---

## 5. Content Source

Adapted from CCSCNE-2026 workshop materials, migrated to plcc-ng syntax.
Activity3 (prefix expression evaluator) is the semantic activity and
provides solutions in Python, Java, and JavaScript.

---

## 6. Also Validates

This repo is a live test of plcc-ng, plcc-ng-devcontainer, and the
migration instructions. Issues found are filed in `dev-docs/issues/`
with a Target repo note for upstream filing.
```

- [ ] **Step 7: Create topic directory skeletons**

```bash
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/01-lexical/activity
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/01-lexical/solution
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/02-syntactic/activity
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/02-syntactic/solution
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/03-semantic/activity
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/python
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/java
mkdir -p /Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/javascript
```

- [ ] **Step 8: Initial commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add .
git commit -m "chore: scaffold repo with devcontainer, issue tracking, and directory structure"
```

---

### Task 2: Root README

**Files:**
- Create: `README.md`

- [ ] **Step 1: Write README.md**

Create `/Users/stoney/Workspace/plcc-ng-demo/README.md`:

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add README.md
git commit -m "docs: add root README with two-path navigation"
```

---

### Task 3: 01-lexical

**Files:**
- Create: `01-lexical/activity/spec.plcc`
- Create: `01-lexical/activity/testprog.txt`
- Create: `01-lexical/solution/spec.plcc`
- Create: `01-lexical/solution/testprog.txt`
- Create: `01-lexical/README.md`
- Create: `01-lexical/solution/README.md`

The activity is adapted from CCSCNE-2026 Activity1. Students define scanner
tokens using regular expressions. The plcc-ng tool is `plcc-scan`.

**Key migration changes from original `idents.plcc`:**
- `#.*$` → `#.*` (remove trailing `$`)
- `\s+` → `\s+` (unchanged)
- `[A-Za-z%$@][_A-Za-z%$@]*` → unchanged (Python regex compatible)
- No build step: old `plccmk -c idents.plcc` + `scan` → now just `plcc-scan -s spec.plcc`
- No syntactic or semantic sections needed; keep single `%` separator at end or omit

- [ ] **Step 1: Create activity starter spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/01-lexical/activity/spec.plcc`:
```
skip WHITESPACE '\s+'

# Skip comments: lines starting with '#'
skip COMMENT '#.*'

token IDENT '[A-Za-z%$@][_A-Za-z%$@]*'

# Add your new token definitions below:
# - INTEGER: unsigned decimal integers
# - IF: the reserved word "if"
# - ELSE: the reserved word "else"
#
# IMPORTANT: Place IF and ELSE before IDENT so reserved words take priority.
```

- [ ] **Step 2: Create activity test program**

Create `/Users/stoney/Workspace/plcc-ng-demo/01-lexical/activity/testprog.txt`:
```
# Test existing token definitions.
%
$%
@@%$%Hi_there

Skip_the_underscore
_rest_is_OK

# Test the tokens you add.
if
else
biff welser ifato elseanor
0
012
1537

Bad_decimal_point_coming_up
49.68

Is this OK
13fourteen15sixteen # Is whitespace necessary?
```

- [ ] **Step 3: Create solution spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/01-lexical/solution/spec.plcc`:
```
skip WHITESPACE '\s+'

# Skip comments: lines starting with '#'
skip COMMENT '#.*'

token INTEGER '\d+'
token IF 'if'
token ELSE 'else'

token IDENT '[A-Za-z%$@][_A-Za-z%$@]*'
```

- [ ] **Step 4: Copy solution test program**

Create `/Users/stoney/Workspace/plcc-ng-demo/01-lexical/solution/testprog.txt`:
```
# Test existing token definitions.
%
$%
@@%$%Hi_there

Skip_the_underscore
_rest_is_OK

# Test the new tokens.
if
else
biff welser ifato elseanor
0
012
1537

Bad_decimal_point
49.68

Is this OK
13fourteen15sixteen # Is whitespace necessary?
```

- [ ] **Step 5: Write activity README**

Create `/Users/stoney/Workspace/plcc-ng-demo/01-lexical/README.md`:
```markdown
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
doesn't match a token is reported as an `ERROR`.

## Check your work

Once your scanner recognizes `if`, `else`, and integers correctly, check your
results against the solution:

```
plcc-scan -s solution/spec.plcc < activity/testprog.txt
```

See `solution/README.md` for a walkthrough of the solution.
```

- [ ] **Step 6: Write solution test-drive README**

Create `/Users/stoney/Workspace/plcc-ng-demo/01-lexical/solution/README.md`:
```markdown
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

You should see output like:
```
INTEGER(0)
IF(if)
ELSE(else)
...
IDENT(biff)
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
```

- [ ] **Step 7: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add 01-lexical/
git commit -m "feat: add 01-lexical activity and solution"
```

---

### Task 4: 02-syntactic

**Files:**
- Create: `02-syntactic/activity/spec.plcc`
- Create: `02-syntactic/activity/testprog.txt`
- Create: `02-syntactic/solution/spec.plcc`
- Create: `02-syntactic/solution/testprog.txt`
- Create: `02-syntactic/README.md`
- Create: `02-syntactic/solution/README.md`

Adapted from CCSCNE-2026 Activity2. Students fix an ambiguous LL(1) grammar
for if-statements by adding an `ENDIF` keyword. The plcc-ng tool is
`plcc-parse`. No semantic section needed.

**Key migration changes:**
- Nonterminals to PascalCase: `<prog>` → `<Prog>`, `<stmts>` → `<Stmts>`, `<stmt>` → `<Stmt>`, `<elsePart>` → `<ElsePart>`, `<expr>` → `<Expr>`
- LHS class notation: `<stmt>Assign` → `<Stmt:Assign>`, `<stmt>IfStmt` → `<Stmt:IfStmt>`, `<elsePart>EPnada` → `<ElsePart:EPnada>`, `<elsePart>EPelse` → `<ElsePart:EPelse>`, `<expr>Var` → `<Expr:Var>`, `<expr>Const` → `<Expr:Const>`
- Old command `plccmk -c if-stmt.plcc` then `parse` → now just `plcc-parse -s spec.plcc`
- `#.*` not `#.*$`

- [ ] **Step 1: Create activity starter spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/02-syntactic/activity/spec.plcc`:
```
skip WHITESPACE '\s+'
skip COMMENT '#.*'

# Reserved words must come before IDENT
token IF    'if'
token THEN  'then'
token ELSE  'else'
token ASSGN '='
token EOP   '\.'

token IDENT    '[A-Za-z%$@][_A-Za-z%$@]*'
token INTEGER  '[0-9]+'

%

# This grammar is ambiguous — the LL(1) parser cannot tell apart
# an if-statement with and without an else clause.
# Fix it by adding an ENDIF token (add the token above) and requiring
# every if-statement to end with it.

<Prog>            ::= <Stmts:stmts> EOP
<Stmts>           **= <Stmt:stmt>

<Stmt:Assign>     ::= <IDENT:ident> ASSGN <Expr:expr>

<Stmt:IfStmt>     ::= IF <Expr:expr> THEN <Stmt:stmt> <ElsePart:elsePart>
<ElsePart:EPnada> ::=
<ElsePart:EPelse> ::= ELSE <Stmt:stmt>

<Expr:Var>        ::= <IDENT:ident>
<Expr:Const>      ::= <INTEGER:integer>
```

- [ ] **Step 2: Create activity test program**

Create `/Users/stoney/Workspace/plcc-ng-demo/02-syntactic/activity/testprog.txt`:
```
# Indentation is here only for readability.

a = 2
if a then a = 3 endif

b = 4
if b then b = 5 else b = 6 endif

c = 7
d = 8
if c then
    if d then c = 9 endif
else
    d = 10
endif

.
```

- [ ] **Step 3: Create solution spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/02-syntactic/solution/spec.plcc`:
```
skip WHITESPACE '\s+'
skip COMMENT '#.*'

# Reserved words must come before IDENT
token IF    'if'
token THEN  'then'
token ELSE  'else'
token ENDIF 'endif'
token ASSGN '='
token EOP   '\.'

token IDENT    '[A-Za-z%$@][_A-Za-z%$@]*'
token INTEGER  '[0-9]+'

%

<Prog>            ::= <Stmts:stmts> EOP
<Stmts>           **= <Stmt:stmt>

<Stmt:Assign>     ::= <IDENT:ident> ASSGN <Expr:expr>

<Stmt:IfStmt>     ::= IF <Expr:expr> THEN <Stmt:stmt> <ElsePart:elsePart> ENDIF
<ElsePart:EPnada> ::=
<ElsePart:EPelse> ::= ELSE <Stmt:stmt>

<Expr:Var>        ::= <IDENT:ident>
<Expr:Const>      ::= <INTEGER:integer>
```

- [ ] **Step 4: Copy solution test program**

Create `/Users/stoney/Workspace/plcc-ng-demo/02-syntactic/solution/testprog.txt`:
```
# Indentation is here only for readability.

a = 2
if a then a = 3 endif

b = 4
if b then b = 5 else b = 6 endif

c = 7
d = 8
if c then
    if d then c = 9 endif
else
    d = 10
endif

e = 11
f = 12
if e then
    if f then
        e = 13
    else
        f = 14
    endif
endif

.
```

- [ ] **Step 5: Write activity README**

Create `/Users/stoney/Workspace/plcc-ng-demo/02-syntactic/README.md`:
```markdown
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

```
plcc-parse -s activity/spec.plcc < activity/testprog.txt
```

If the grammar is valid LL(1), a successful parse prints `OK`. An ambiguous
or incorrect grammar will produce an error message.

Parse interactively (type one program at a time, press Ctrl-D to finish):

```
plcc-parse -s activity/spec.plcc
```

To trace the parse step-by-step (useful for debugging):

```
plcc-parse -s activity/spec.plcc -t < activity/testprog.txt
```

## Check your work

```
plcc-parse -s solution/spec.plcc < activity/testprog.txt
```

See `solution/README.md` for a walkthrough.
```

- [ ] **Step 6: Write solution test-drive README**

Create `/Users/stoney/Workspace/plcc-ng-demo/02-syntactic/solution/README.md`:
```markdown
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
```

- [ ] **Step 7: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add 02-syntactic/
git commit -m "feat: add 02-syntactic activity and solution"
```

---

### Task 5: 03-semantic — activity

**Files:**
- Create: `03-semantic/activity/spec.plcc`
- Create: `03-semantic/activity/testprog1.txt`
- Create: `03-semantic/activity/testprog2.txt`
- Create: `03-semantic/README.md`

Adapted from CCSCNE-2026 Activity3 (prefix expression evaluator). Students
complete the semantic section of a prefix arithmetic interpreter in Python.
The `_run()` method and `Add` class are provided; students implement `Sub`,
`Mul`, `Div`, and add unary negation.

**Key migration changes from `Activity3/prefix.plcc`:**
- `#.*$` → `#.*`
- `<expr>Const` → `<Expr:Const>`, `<expr>Add` → `<Expr:Add>`, etc.
- `<expr>opnd1` (RHS field) → `<Expr:opnd1>`, `<expr>opnd2` → `<Expr:opnd2>`
- `<INTEGER>` (no field) → `<INTEGER:integer>` (with field, for semantic access)
- Old Java `%%{...%%}` blocks → Python `%%%...%%%` blocks
- `public void $run()` → `def _run(self):`
- `Integer.parseInt(integer.toString())` → `int(self.integer.lexeme)`
- `opnd1.evaluate()` → `self.opnd1.evaluate()`
- Language header: `% Java` → `Python` (just the word, no `%` prefix)

- [ ] **Step 1: Create activity starter spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/activity/spec.plcc`:
```
skip WHITESPACE '\s+'
skip COMMENT '#.*'

# Why do the first three patterns need backslash escapes?
token ADDOP '\+'
token SUBOP '\-'
token MULOP '\*'
token DIVOP '/'
# Add a new token for unary negation (_) here.

token INTEGER '[0-9]+'

%

<Expr:Const>  ::= <INTEGER:integer>
<Expr:Add>    ::= ADDOP <Expr:opnd1> <Expr:opnd2>
<Expr:Sub>    ::= SUBOP <Expr:opnd1> <Expr:opnd2>
<Expr:Mul>    ::= MULOP <Expr:opnd1> <Expr:opnd2>
<Expr:Div>    ::= DIVOP <Expr:opnd1> <Expr:opnd2>
# Add a grammar rule for unary negation here.

%

Python

Expr
%%%
def _run(self):
    print(self.evaluate())

# This dummy implementation lets you test before all classes are complete.
# Replace it with `raise NotImplementedError` once you're ready.
def evaluate(self):
    return 999
%%%

Const
%%%
def evaluate(self):
    return int(self.integer.lexeme)
%%%

Add
%%%
def evaluate(self):
    return self.opnd1.evaluate() + self.opnd2.evaluate()
%%%

# Complete Sub, Mul, and Div below following the Add pattern.
# Then add support for unary negation.
```

- [ ] **Step 2: Copy test programs**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/activity/testprog1.txt`:
```
# These should work correctly right away.
3
+ 3 9

# These will return 999 until you implement Sub, Mul, and Div.
- 2 1
* / 45 5 - 3 10
```

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/activity/testprog2.txt`:
```
3
+ 3 9
- 2 1
- 3 4
* 18 5
/ 12 2
* + 12 6 5
* / 45 5 - 3 10
* / 45 5 _ - 3 10
```

- [ ] **Step 3: Write activity README**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/README.md`:
```markdown
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

1. `Sub`, `Mul`, and `Div` — follow the `Add` pattern
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
```

- [ ] **Step 4: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add 03-semantic/activity/ 03-semantic/README.md
git commit -m "feat: add 03-semantic activity"
```

---

### Task 6: 03-semantic — Python solution

**Files:**
- Create: `03-semantic/solution/python/spec.plcc`
- Create: `03-semantic/solution/python/testprog.txt`

Complete Python implementation. All operators plus unary negation.

- [ ] **Step 1: Create Python solution spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/python/spec.plcc`:
```
skip WHITESPACE '\s+'
skip COMMENT '#.*'

token ADDOP '\+'
token SUBOP '\-'
token MULOP '\*'
token DIVOP '/'
token UMIOP '_'

token INTEGER '[0-9]+'

%

<Expr:Const>  ::= <INTEGER:integer>
<Expr:Add>    ::= ADDOP <Expr:opnd1> <Expr:opnd2>
<Expr:Sub>    ::= SUBOP <Expr:opnd1> <Expr:opnd2>
<Expr:Mul>    ::= MULOP <Expr:opnd1> <Expr:opnd2>
<Expr:Div>    ::= DIVOP <Expr:opnd1> <Expr:opnd2>
<Expr:Umi>    ::= UMIOP <Expr:opnd1>

%

Python

Expr
%%%
def _run(self):
    print(self.evaluate())

def evaluate(self):
    raise NotImplementedError
%%%

Const
%%%
def evaluate(self):
    return int(self.integer.lexeme)
%%%

Add
%%%
def evaluate(self):
    return self.opnd1.evaluate() + self.opnd2.evaluate()
%%%

Sub
%%%
def evaluate(self):
    return self.opnd1.evaluate() - self.opnd2.evaluate()
%%%

Mul
%%%
def evaluate(self):
    return self.opnd1.evaluate() * self.opnd2.evaluate()
%%%

Div
%%%
def evaluate(self):
    return self.opnd1.evaluate() // self.opnd2.evaluate()
%%%

Umi
%%%
def evaluate(self):
    return -self.opnd1.evaluate()
%%%
```

- [ ] **Step 2: Create test program**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/python/testprog.txt`:
```
3
+ 3 9
- 2 1
- 3 4
_3
* 18 5
/ 12 2
* + 12 6 5
_ * + 12 6 5
* / 45 5 - 3 10
* / 45 5 _ - 3 10
```

Expected output (one result per line):
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
63
-63
```

- [ ] **Step 3: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add 03-semantic/solution/python/
git commit -m "feat: add 03-semantic Python solution"
```

---

### Task 7: 03-semantic — Java solution

**Files:**
- Create: `03-semantic/solution/java/spec.plcc`
- Create: `03-semantic/solution/java/testprog.txt`

Same lexical and syntactic sections as Python solution. Semantic section uses
Java with `% Java` → `Java` header, `public` method declarations, `_run()`
returning void, `integer.toString()` for token lexeme access.

- [ ] **Step 1: Create Java solution spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/java/spec.plcc`:
```
skip WHITESPACE '\s+'
skip COMMENT '#.*'

token ADDOP '\+'
token SUBOP '\-'
token MULOP '\*'
token DIVOP '/'
token UMIOP '_'

token INTEGER '[0-9]+'

%

<Expr:Const>  ::= <INTEGER:integer>
<Expr:Add>    ::= ADDOP <Expr:opnd1> <Expr:opnd2>
<Expr:Sub>    ::= SUBOP <Expr:opnd1> <Expr:opnd2>
<Expr:Mul>    ::= MULOP <Expr:opnd1> <Expr:opnd2>
<Expr:Div>    ::= DIVOP <Expr:opnd1> <Expr:opnd2>
<Expr:Umi>    ::= UMIOP <Expr:opnd1>

%

Java

Expr
%%%
    public void _run() {
        System.out.println(this.evaluate());
    }
    public abstract int evaluate();
%%%

Const
%%%
    public int evaluate() {
        return Integer.parseInt(integer.toString());
    }
%%%

Add
%%%
    public int evaluate() {
        return opnd1.evaluate() + opnd2.evaluate();
    }
%%%

Sub
%%%
    public int evaluate() {
        return opnd1.evaluate() - opnd2.evaluate();
    }
%%%

Mul
%%%
    public int evaluate() {
        return opnd1.evaluate() * opnd2.evaluate();
    }
%%%

Div
%%%
    public int evaluate() {
        return opnd1.evaluate() / opnd2.evaluate();
    }
%%%

Umi
%%%
    public int evaluate() {
        return -opnd1.evaluate();
    }
%%%
```

- [ ] **Step 2: Copy test program**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/java/testprog.txt`
with the same content as the Python testprog.txt (same input, same expected output).

```
3
+ 3 9
- 2 1
- 3 4
_3
* 18 5
/ 12 2
* + 12 6 5
_ * + 12 6 5
* / 45 5 - 3 10
* / 45 5 _ - 3 10
```

- [ ] **Step 3: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add 03-semantic/solution/java/
git commit -m "feat: add 03-semantic Java solution"
```

---

### Task 8: 03-semantic — JavaScript solution

**Files:**
- Create: `03-semantic/solution/javascript/spec.plcc`
- Create: `03-semantic/solution/javascript/testprog.txt`

Same lexical and syntactic sections. Semantic section uses JavaScript.
JavaScript in plcc-ng uses `JavaScript` as the language header. Methods
use `methodName() { ... }` syntax (no `def`, no `self`, use `this`).
Token lexeme access: `this.integer.toString()` (same pattern as Java).

**Note:** The JavaScript semantic section syntax is a best-effort migration.
File any issues found in Codespaces in `dev-docs/issues/` with
**Target repo:** `ourPLCC/plcc-ng`.

- [ ] **Step 1: Create JavaScript solution spec**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/javascript/spec.plcc`:
```
skip WHITESPACE '\s+'
skip COMMENT '#.*'

token ADDOP '\+'
token SUBOP '\-'
token MULOP '\*'
token DIVOP '/'
token UMIOP '_'

token INTEGER '[0-9]+'

%

<Expr:Const>  ::= <INTEGER:integer>
<Expr:Add>    ::= ADDOP <Expr:opnd1> <Expr:opnd2>
<Expr:Sub>    ::= SUBOP <Expr:opnd1> <Expr:opnd2>
<Expr:Mul>    ::= MULOP <Expr:opnd1> <Expr:opnd2>
<Expr:Div>    ::= DIVOP <Expr:opnd1> <Expr:opnd2>
<Expr:Umi>    ::= UMIOP <Expr:opnd1>

%

JavaScript

Expr
%%%
_run() {
    console.log(this.evaluate());
}

evaluate() {
    throw new Error('Not implemented');
}
%%%

Const
%%%
evaluate() {
    return parseInt(this.integer.toString());
}
%%%

Add
%%%
evaluate() {
    return this.opnd1.evaluate() + this.opnd2.evaluate();
}
%%%

Sub
%%%
evaluate() {
    return this.opnd1.evaluate() - this.opnd2.evaluate();
}
%%%

Mul
%%%
evaluate() {
    return this.opnd1.evaluate() * this.opnd2.evaluate();
}
%%%

Div
%%%
evaluate() {
    return Math.trunc(this.opnd1.evaluate() / this.opnd2.evaluate());
}
%%%

Umi
%%%
evaluate() {
    return -this.opnd1.evaluate();
}
%%%
```

- [ ] **Step 2: Copy test program**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/javascript/testprog.txt`
with the same content as the Python testprog.txt.

```
3
+ 3 9
- 2 1
- 3 4
_3
* 18 5
/ 12 2
* + 12 6 5
_ * + 12 6 5
* / 45 5 - 3 10
* / 45 5 _ - 3 10
```

- [ ] **Step 3: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add 03-semantic/solution/javascript/
git commit -m "feat: add 03-semantic JavaScript solution"
```

---

### Task 9: 03-semantic — solution README (test drive entry point)

**Files:**
- Create: `03-semantic/solution/README.md`

This is the main test drive document linked from the root README. It walks
through all three plcc-ng tools (`plcc-scan`, `plcc-parse`, `plcc-rep`) and
lets the reader pick their preferred semantic language.

- [ ] **Step 1: Write solution README**

Create `/Users/stoney/Workspace/plcc-ng-demo/03-semantic/solution/README.md`:
```markdown
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

You'll see each token printed individually:

```
INTEGER(3)
ADDOP(+)
INTEGER(3)
INTEGER(9)
...
```

Try scanning interactively — type an expression and press Enter:

```
plcc-scan -s python/spec.plcc
```

(Press Ctrl-D on a new line to exit.)

---

## Step 2: Parse — structure

`plcc-parse` checks that the token stream matches the grammar and prints `OK`
if it does.

```
plcc-parse -s python/spec.plcc < python/testprog.txt
```

Trace the parse tree being built with `-t`:

```
plcc-parse -s python/spec.plcc -t < python/testprog.txt
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
63
-63
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
```

- [ ] **Step 2: Commit**

```bash
cd /Users/stoney/Workspace/plcc-ng-demo
git add 03-semantic/solution/README.md
git commit -m "docs: add 03-semantic solution test-drive README"
```

---

## Verification (must be done in GitHub Codespaces)

Since plcc-ng is not installed locally, all functional verification must be
done by opening the repo in GitHub Codespaces after pushing it to GitHub
under `ourPLCC/plcc-ng-demo`.

Verification checklist:
- [ ] Codespace opens without errors (devcontainer image pulls and runs)
- [ ] `plcc-scan -s 01-lexical/solution/spec.plcc < 01-lexical/solution/testprog.txt` produces correct token output
- [ ] `plcc-scan -s 01-lexical/solution/spec.plcc` works interactively
- [ ] `plcc-parse -s 02-syntactic/solution/spec.plcc < 02-syntactic/solution/testprog.txt` prints `OK`
- [ ] `plcc-parse -s 02-syntactic/solution/spec.plcc` works interactively
- [ ] `plcc-parse -s 02-syntactic/solution/spec.plcc -t < 02-syntactic/solution/testprog.txt` shows parse trace
- [ ] `plcc-rep -s 03-semantic/solution/python/spec.plcc < 03-semantic/solution/python/testprog.txt` produces correct output
- [ ] `plcc-rep -s 03-semantic/solution/java/spec.plcc < 03-semantic/solution/java/testprog.txt` produces correct output
- [ ] `plcc-rep -s 03-semantic/solution/javascript/spec.plcc < 03-semantic/solution/javascript/testprog.txt` produces correct output
- [ ] `plcc-rep -s 03-semantic/solution/python/spec.plcc` works interactively
- [ ] Activity starter files give clear errors/incomplete output (not full solutions)
- [ ] Any issues found are filed in `dev-docs/issues/` with Target repo noted
```
