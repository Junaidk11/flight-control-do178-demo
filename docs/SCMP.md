# Software Configuration Management Plan (SCMP)

> Project: **Pitch Hold Flight Control System**  
> Standard Context: **DO-178C (DAL C)**  
> Repo: `flight-control-do178-demo`  
> Owner: `[Your Name]` • CM Role: `[Your Name / Reviewer]` • QA/SQAR: `[Independent Reviewer]`

**Important Note:** QA should be independent of the CM and the developer.

---

## 1. Purpose & Scope
This SCMP defines the **configuration identification**, **change control**, **status accounting**, **build/release**, **problem reporting**, **audit**, **access control**, and **records retention** practices for this project.  
Goal: ensure every released software item is **uniquely identified, controlled, reproducible, and traceable** to requirements and verification evidence.

---

## 2. References & Terms
- DO-178C (Configuration Management & Quality Assurance objectives)  
- Project plans: **PSAC**, **SDP**, **SVP**, **SQAP**  
- Terms: **CI**(Configuration Item), **CR**(Change Request), **FCA**(Functional Configuration Audit), **PCA**(Physical Configuration Audit)

---

## 3. CM Strategy (Overview)
- **Everything in Git** (code, docs, tests, templates, checklists)
- **Branch protection** for `main`; changes via **Pull Requests** with mandatory review
- **Baselines** created via immutable **signed tags**
- **Releases** produce archived artifacts (source snapshot, coverage, trace matrix, checksums)
- **Issues**/PRs track all CRs; each change links to requirement(s) & test(s)

---

## 4. Configuration Items (CIs)
Tracked & versioned CIs include (non-exhaustive):

| CI Category | Path / ID | Description | Trace Links |
|---|---|---|---|
| Plans | `docs/PSAC.md`, `docs/SDP.md`, `docs/SVP.md`, `docs/SCMP.md`, `docs/SQAP.md` | Life-cycle plans | PSAC §x refs |
| Requirements | `docs/Requirements.md` | HLR (and LLR when added) | Trace Matrix |
| Design | `design/*` | Architecture / state machines | Req IDs |
| Source | `src/*` | C implementation | LLRs / HLRs |
| Tests | `tests/*` | Unit tests, procedures, results | Req IDs |
| Coverage | `tests/coverage_report/*` | Coverage evidence | Test IDs |
| Reviews | `reviews/*` | Checklists, logs, approvals | Artifact IDs |
| Traceability | `docs/TraceabilityMatrix.xlsx` | End-to-end trace | All above |
| Releases | `releases/*` (exported) | Packaged baselines | Tag / checksum |

---

## 5. Identification (Naming, Versioning, Structure)
**Repository structure** (canonical):
```
docs/ 
design/ 
src/ 
tests/ 
reviews/ 
releases/
```

**File naming**:  
- Plans: `docs/<PLAN>.md` (e.g., `SVP.md`)  
- Requirements: `docs/Requirements.md` with IDs `HLR-<n>`, `LLR-<n>`  
- Tests: `tests/TC-<id>_<short-name>.c`  
- Reviews: `reviews/2025-08-XX_<artifact>_review.md`

**Versioning**:  
- **Tags**: `vMAJOR.MINOR.PATCH` (e.g., `v0.2.0`)  
- **Pre-release suffix** (optional): `-rc.1`  
- **Artifacts embed version** in headers (top of .md / source banners)

---

## 6. Change Control (CR Workflow)
All changes flow via GitHub **Issue → PR → Review → Merge**.

**Workflow:**
1. Open **Issue** with template (Change Request) and link affected Req IDs (HLR/LLR).
2. Create feature branch: `feature/<short-desc>` or `fix/<short-desc>`.
3. Commit with structured messages: `feat(pid): implement anti-windup (HLR-2, LLR-2.3, TC-017)`
4. Open **PR** referencing Issue; attach review checklist.
5. **Independent Review** (not the author) approves; CI green.
6. Merge → Close Issue; update **Traceability Matrix**.

**Emergency changes**: use `hotfix/*` + expedited review (document rationale in PR).

**Deviation handling**: document in PR + **SQAP** waiver entry; link to corrective action.

---

## 7. Baselines
- **Development Baseline**: `main` at milestone tags.
- **Test Baseline**: tagged when all planned tests pass & coverage ≥ target.
- **Release Baseline**: immutable **signed tag** with packaged evidence.

**Tagging**:
```
git tag -s v0.2.0 -m "DAL C release baseline: HLR 1–5 verified"
git push origin v0.2.0
```

---

## 8. Build & Release Management
**Build reproducibility**:
- Document tool versions in `docs/BuildEnv.md` (compiler, OS, libraries).
- Scripted build (e.g., `Makefile` or `CMake`) + deterministic flags.
- Optional: Dockerfile to capture environment.

**Release package contents (`releases/vX.Y.Z/`)**:
- `source_snapshot.zip` (or tag reference)
- `test_results/` (logs, junit, etc.)
- `coverage_report/`
- `docs/TraceabilityMatrix.xlsx`
- `reviews/` logs
- `checksums.txt` (SHA256)
- `RELEASE_NOTES.md`

Generate checksums:
```
shasum -a 256 releases/v0.2.0/* > releases/v0.2.0/checksums.txt
```

---

## 9. Status Accounting
- **Issues/PRs** are the system of record for CRs.
- Labels: `req`, `design`, `code`, `test`, `doc`, `critical`, `bug`, `waiver`.
- **Dashboards**: GitHub Projects board reflecting status (ToDo/In-Progress/Review/Done).
- **Traceability Matrix** updated per merge (HLR/LLR ↔ Code ↔ Tests).

---

## 10. Problem Reporting & Corrective Action
**Issue types**: `bug`, `nonconformance`, `process`.  
**Lifecycle**: Open → Triage (severity, impact, containment) → Fix → Verify → Close.  
**Root Cause (RCA)** & **CAPA** recorded in the issue using template.

---

## 11. Configuration Audits
**FCA (Functional Configuration Audit)** — verifies functionality against requirements:
- All HLR/LLR covered by tests; results PASS
- Coverage meets target (statement+decision for DAL C)
- Open problems triaged with acceptable risk

**PCA (Physical Configuration Audit)** — verifies the release package:
- Tag matches source
- All planned deliverables present & checksums verified
- Versions and documents consistent

Use `reviews/FCA_checklist.md` & `reviews/PCA_checklist.md`.

---

## 12. Tools & Environment Control
- Tool list & versions: `docs/BuildEnv.md`
- Critical tools: Compiler, Unit test framework (Unity/CMock), Coverage (gcov)
- Changes to tools require Issue + PR + environment update
- **Tool qualification**: Not claimed; results validated by reviews/tests

---

## 13. Access Control & Backups
- **Branch protection**: `main` requires:
  - 1+ reviewer approval
  - Status checks passing
  - Linear history (rebase/merge rules)
- **Secrets**: none stored in repo
- **Backups**: remote Git hosting + periodic local clone/archive of tags and releases

---

## 14. Records & Retention
- Keep tags, releases, reviews, test results, coverage, and traceability **for ≥ 3 years** (or per job/customer requirement).
- Store release zips and checksums in `releases/` and an external backup (e.g., cloud drive).

---

## 15. Roles & Responsibilities
- **Developer**: Implements changes, updates trace matrix, writes tests.
- **Reviewer (Independent)**: Reviews req/design/code/tests; completes checklists.
- **CM (You)**: Enforces SCMP, baselines, tags, packaging.
- **QA/SQAR (Independent)**: Verifies compliance with plans, audits FCA/PCA.

---

## 16. Checklists (Mini)
**Review checklist (all artifacts):**
- [ ] Correctness vs. referenced requirements
- [ ] Clarity & consistency
- [ ] Trace links updated
- [ ] Style/standard adherence (MISRA where applicable)
- [ ] Tests updated/added
- [ ] No dead/unreachable code (for code reviews)

---

## Appendix A — Git Workflow (Example)

```
# Create branch
git checkout -b feature/pid-anti-windup

# Commit
git commit -m "feat(pid): add anti-windup (HLR-2, LLR-2.3, TC-017)"

# Push
git push -u origin feature/pid-anti-windup
```

- Open PR, request review, ensure CI green
- After approval: Merge PR -> main

---

## Appendix B — Templates

### B.1 Change Request (Issue Template)
```
Title: [CR] <short description>

Affected Items: HLR-__, LLR-__, Modules: src/__, Tests: TC-__
Rationale / Problem:
Proposed Change:
Impact Analysis:
Verification:
Links: PR #__, Commit __
```

### B.2 Pull Request Template
```
## Summary
What changed and why.

## Linked Items
Issues: #__
Requirements: HLR-__, LLR-__

## Verification
- [ ] Unit tests added/updated
- [ ] All tests passing
- [ ] Coverage not reduced
- [ ] Traceability updated

## Review
- [ ] Independent review completed
Checklist attached in `reviews/`.

```

### B.3 Release Notes (RELEASE_NOTES.md)
```
# vX.Y.Z — YYYY-MM-DD
Scope: [DAL C baseline]

Changes:
- HLR-1..5 verified; new LLR-2.3
- Bug fixes: #12

Artifacts:
- coverage_report/ (stmt, decision)
- tests/results/
- docs/TraceabilityMatrix.xlsx

Checksums:
- see checksums.txt

```

### Appendix C — Naming Standards
```
Branches: feature/<desc>, fix/<desc>, hotfix/<desc>

Commits: <type>(<area>): <summary> (Refs: HLR-__, TC-__)

type: feat, fix, refactor, test, docs, chore

Tags: vMAJOR.MINOR.PATCH[-rc.n]
```

## Appendix D — CI Setup (GitHub Actions)

This CI pipeline does four main things on every push/PR to `main`:

1) **Build** the project (CMake example; switch to Make if you prefer).  
2) **Run unit tests** (expects tests to live under `tests/` and produce an exit code).  
3) **Generate code coverage** with `gcovr` (statement/decision coverage).  
4) **Upload artifacts** (test logs, coverage HTML) for inspection.

> Tip: keep compilation flags in your build files so coverage works:  
> `-O0 -g -fprofile-arcs -ftest-coverage`

### File: `.github/workflows/ci.yml`