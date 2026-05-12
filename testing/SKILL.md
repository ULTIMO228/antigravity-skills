---
name: testing
description: >
  Write tests for completed features. Detects stack (Python/TS),
  loads correct reference, follows AAA pattern. Min 3 tests per function:
  happy path + edge case + error case. Runs tests after writing.
---

# testing — Multi-Stack Test Writer

## Step 0: Stack detection
Read `.ai/SKILL_CONFIG.md` → load matching reference:
- Python → read `references/python-pytest.md`
- TypeScript → read `references/ts-vitest.md`

## Procedure

### 1. Classify test type
| Signal | Type | Priority |
|--------|------|----------|
| Isolated fn, pure logic | unit | highest |
| API/module interaction | integration | medium |
| User journey, full flow | e2e | lowest |

### 2. Detect test runner
Python: `pyproject.toml` → pytest section? | `pytest.ini`?
TS: `package.json` → vitest | jest | playwright
None found → propose install.

### 3. Placement
Follow existing convention:
- Python: `tests/test_<module>.py` | `tests/<module>/test_<feature>.py`
- TS: `__tests__/` | `*.test.ts` | `tests/`
No convention → propose structure.

### 4. Write tests (AAA pattern)
Min 3 tests per function:
- ✅ Happy path (valid input → expected output)
- ⚠️ Edge case (boundary, null, empty, zero, max)
- ❌ Error case (invalid input → proper error)

Test names: descriptive, in Russian (for readability).

### 5. Rules
| Rule | Why |
|------|-----|
| Test behavior, not implementation | Survives refactoring |
| Each test independent | No order dependency |
| Mock external deps only | DB, API, file system |
| Don't chase 100% coverage | Focus business logic ≥ 80% |
| No test logic | No if/else in tests |

### 6. Run
Execute test runner. All must pass.
Coverage: run with `--cov` flag if available.

→ After: `code-review` | `git-commit`
