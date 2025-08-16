# Continuous Integration (CI) Overview

This project uses **GitHub Actions** to enforce build reproducibility and verification evidence.

## What the pipeline does
1. **Build** (CMake) with coverage flags `--coverage -O0 -g`
2. **Run unit tests** (CTest or test runner)
3. **Generate coverage** with `gcovr` (XML + HTML)
4. **Upload artifacts** (coverage, logs) to each run

## Files
- Workflow: `.github/workflows/ci.yml`
- Coverage output: `build/coverage/` (HTML report: `index.html`)
- Test logs: `build/Testing/` or `tests/results/` (depending on setup)

## Branch protection (required checks)
- `build-test-coverage`
- `static-analysis` (optional)

## Change control
- CI workflow is a **Configuration Item**; changes require PR + independent review.
- Tool versions listed in `docs/BuildEnv.md`; updates follow SCMP change control.

## Notes
- Excludes `tests/` and `third_party/` from coverage (adjust as needed).
- Failing tests or analysis **block merges** to `main`.
