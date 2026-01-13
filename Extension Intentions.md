# Extension Intentions

Each project can define extension intentions beyond the core F, B, R, D. This document describes the extensions used at Quatico.

| Code | Name | Use | Alternatives |
|------|------|-----|--------------|
| `T` | Test-only | Alter automated tests without altering functionality. May include code that just throws `NotImplementedException`. | Use `F` or `B` depending on what the test validates. Use `R` for refactoring within test code. |
| `E` | Environment | Non-code changes affecting dev setup, tooling, or configuration that don't affect program behavior. | Treat the environment as a product where users are team members. |
| `A` | Automated | Tool-assisted changes: IDE refactoring (rename, extract, move), search & replace, formatters, linters with auto-fix, AI-assisted refactoring. | Use the intention matching the underlying goal (`R`, `F`, etc.). Prefer `A` when the method is more relevant than the goal, or changes span multiple intentions. |
| `C` | Comment | Comment-only changes. Does not include comments visible to doc-generation tools (JSDoc, JavaDoc). | Use `D`. |
| `S` | Spec | Changes to formal specs or design docs kept in source control. | Use `D`, keep specs outside source control, or use test suite as spec. |
| `*` | Unknown | Multiple unrelated changes, just getting it checked in. Usually `***`. | Require each commit to have exactly one intention. |

## Risk Level Examples

### Test-only (T)

| Code | Approach |
|------|----------|
| `t` | Refactoring purely within test code |
| `T` | New test for existing behavior, all tests pass |
| `T!!` | New test without running full suite |
| `T**` | Incomplete test, WIP |

### Environment (E)

| Code | Approach |
|------|----------|
| `e` | Config change with no compilation impact |
| `E` | Tooling change, verified working |
| `E!!` | CI/CD change, not fully tested |
| `E**` | Experimenting with build config |

### Automated (A)

| Code | Approach |
|------|----------|
| `a` | IDE rename/extract with type verification, compiler confirms no errors |
| `A` | Tool-assisted with test verification |
| `A!!` | Bulk operation (formatter, linter) manually reviewed |
| `A**` | AI suggestions accepted without review |

### Comment (C)

| Code | Approach |
|------|----------|
| `c` | Comment change, verified byte-identical compilation |
| `C` | Comment change in source file |
| `C!!` | Large comment overhaul |
| `C**` | Draft comments, needs review |

### Spec (S)

| Code | Approach |
|------|----------|
| `s` | Spec update matching implemented behavior |
| `S` | Spec change, implementation follows |
| `S!!` | Spec change, implementation pending |
| `S**` | Draft spec for discussion |
