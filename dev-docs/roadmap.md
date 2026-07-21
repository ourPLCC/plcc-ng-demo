# plcc-ng-demo Roadmap

## Open Issues

- **[#1](issues/001-plcc-parse-no-t-flag-no-ok-output.md)** — Migration doc gaps: `plcc-parse` no longer has `-t`/`OK` output, and `plcc-scan`'s output format changed, neither called out prominently.
- **[#2](issues/002-concurrent-plcc-build-dir-race.md)** — Concurrent `plcc-scan`/`plcc-make` invocations race on the shared `plcc-ng/` build dir and crash with a raw traceback.
- **[#5](issues/005-rename-plcc-rep-to-plcc-eval.md)** — Consider renaming (or aliasing) `plcc-rep` to `plcc-eval` for phase-naming consistency with `plcc-scan`/`plcc-parse`.
