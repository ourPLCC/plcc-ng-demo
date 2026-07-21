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
