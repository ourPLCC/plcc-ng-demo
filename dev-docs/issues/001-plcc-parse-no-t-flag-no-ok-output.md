# 001 - plcc-parse always prints parse tree; no -t flag, no "OK" output

**Type:** docs
**Date:** 2026-07-21

## Description

In old PLCC, `parse` printed `OK` on success and `parse -t` printed a parse
trace. In plcc-ng, `plcc-parse` always prints the parse tree — there is no
`-t` flag and no `OK` output.

The migration guide notes this (`| parse [-t] [-n] [file...] | plcc-parse
[file...] | Always shows parse tree; no -t flag needed |`) but the change is
easy to miss for instructors migrating workshop materials.

## Notes

The migration guide entry is accurate but brief. Instructors expect `-t` and
`OK` from muscle memory and will be confused when neither works.

**Target repo:** `ourPLCC/plcc-ng` (docs) — the migration guide could call
this change out more prominently, e.g. in a "Breaking behavior changes"
callout box.
