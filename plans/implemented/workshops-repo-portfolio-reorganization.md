# Implementation Plan: Portfolio-Ready Workshops Repository

## Overview

Reorganize this repository from a company-specific workshop dump into a polished portfolio repository for multiple workshops. The repository should present Bruno's workshop work professionally to recruiters while preserving the original notebook/document contents. The first workshop will be **Energy Demand Forecasting**. A second root-level placeholder workshop folder will be added for **Harness Engineering**.

## Goals

- Make the repo feel intentional, navigable, and recruiter-friendly.
- Remove the client/company name from the primary folder structure.
- Preserve file contents; only rename/move files and add README documentation.
- Clearly separate student-facing notebooks from solution notebooks.
- Keep large/local data ignored and out of Git.
- Keep the structure scalable for future workshops.

## Current State Summary

Tracked files currently live mostly under:

```text
Hochfrequenz GmbH/
├── block1_regression.ipynb
├── block2_lstm_timeseries.ipynb
├── block3_anomaly_detection.ipynb
├── block4_integrated_project.ipynb
├── data_sources_guide.md
├── hochfrequenz_workshop.md
├── implementation_guide_4organizers.md
├── notebook_templates.md
├── START_HERE.md
├── summary_next_steps.md
└── Shared/
    ├── energy-demand/          # solution notebooks + project files
    └── for-students/           # student notebooks without solutions
```

Important interpretation:

- `Shared/energy-demand/` = notebooks/materials with solutions.
- `Shared/for-students/` = notebooks without solutions.
- Root-level `Hochfrequenz GmbH/*.ipynb` and markdown files appear to be planning/delivery/support material from the workshop package.
- Local data remains ignored at `/Hochfrequenz GmbH/Shared/energy-demand/data/` and should remain out of Git.

## Proposed Target Structure

```text
workshops/
├── README.md
├── .gitignore
├── energy-demand-forecasting/
│   ├── README.md
│   ├── student-materials/
│   │   ├── 01-regression-forecasting.ipynb
│   │   ├── 02-lstm-time-series.ipynb
│   │   ├── 03-anomaly-detection.ipynb
│   │   └── 04-lstm-autoencoder.ipynb
│   ├── solution-materials/
│   │   ├── 01-regression-forecasting.ipynb
│   │   ├── 02-lstm-time-series.ipynb
│   │   ├── 03-anomaly-detection.ipynb
│   │   └── 04-lstm-autoencoder.ipynb
│   ├── project/
│   │   ├── main.py
│   │   ├── pyproject.toml
│   │   ├── uv.lock
│   │   └── assets/
│   │       └── install-jupyter.png
│   ├── docs/
│   │   ├── workshop-curriculum.md
│   │   ├── organizer-implementation-guide.md
│   │   ├── data-sources-guide.md
│   │   ├── notebook-templates.md
│   │   ├── start-here.md
│   │   └── summary-next-steps.md
│   └── archive/
│       ├── block1_regression.ipynb
│       ├── block2_lstm_timeseries.ipynb
│       ├── block3_anomaly_detection.ipynb
│       └── block4_integrated_project.ipynb
├── harness-engineering/
│   └── README.md
└── plans/
    ├── to-implement/
    └── implemented/
```

Notes:

- `archive/` is optional but recommended if the root-level notebooks are older drafts or generated variants. This avoids deleting anything while making the main student/solution paths clear.
- If the root-level notebooks are duplicates of the final solution notebooks, a later cleanup pass can remove them after confirming equivalence. For now, preserve them.
- Use lowercase kebab-case folder names for GitHub readability.

## Rename / Move Map

### Workshop folder

| Current | Target |
|---|---|
| `Hochfrequenz GmbH/` | `energy-demand-forecasting/` |
| `Hochfrequenz GmbH/Shared/for-students/` | `energy-demand-forecasting/student-materials/` |
| `Hochfrequenz GmbH/Shared/energy-demand/` | split into `solution-materials/` and `project/` |

### Student notebooks

| Current | Target |
|---|---|
| `Shared/for-students/Block 1 Regression Notebook.ipynb` | `student-materials/01-regression-forecasting.ipynb` |
| `Shared/for-students/Block 2 LSTM Timeseries Workshop.ipynb` | `student-materials/02-lstm-time-series.ipynb` |
| `Shared/for-students/Block 3 Anomaly Detection Workshop.ipynb` | `student-materials/03-anomaly-detection.ipynb` |
| `Shared/for-students/Block 3.1 LSTM AE.ipynb` | `student-materials/04-lstm-autoencoder.ipynb` |

### Solution notebooks

| Current | Target |
|---|---|
| `Shared/energy-demand/Block 1 Regression Notebook.ipynb` | `solution-materials/01-regression-forecasting.ipynb` |
| `Shared/energy-demand/Block 2 LSTM Timeseries Workshop.ipynb` | `solution-materials/02-lstm-time-series.ipynb` |
| `Shared/energy-demand/Block 3 Anomaly Detection Workshop.ipynb` | `solution-materials/03-anomaly-detection.ipynb` |
| `Shared/energy-demand/Block 3.1 LSTM AE.ipynb` | `solution-materials/04-lstm-autoencoder.ipynb` |

### Project/runtime files

| Current | Target |
|---|---|
| `Shared/energy-demand/main.py` | `project/main.py` |
| `Shared/energy-demand/pyproject.toml` | `project/pyproject.toml` |
| `Shared/energy-demand/uv.lock` | `project/uv.lock` |
| `Shared/energy-demand/.python-version` | `project/.python-version` |
| `Shared/energy-demand/assets/` | `project/assets/` |

### Documentation

| Current | Target |
|---|---|
| `hochfrequenz_workshop.md` | `docs/workshop-curriculum.md` |
| `implementation_guide_4organizers.md` | `docs/organizer-implementation-guide.md` |
| `data_sources_guide.md` | `docs/data-sources-guide.md` |
| `notebook_templates.md` | `docs/notebook-templates.md` |
| `START_HERE.md` | `docs/start-here.md` |
| `summary_next_steps.md` | `docs/summary-next-steps.md` |

### Root-level draft notebooks

| Current | Target |
|---|---|
| `block1_regression.ipynb` | `archive/block1_regression.ipynb` |
| `block2_lstm_timeseries.ipynb` | `archive/block2_lstm_timeseries.ipynb` |
| `block3_anomaly_detection.ipynb` | `archive/block3_anomaly_detection.ipynb` |
| `block4_integrated_project.ipynb` | `archive/block4_integrated_project.ipynb` |

## README Plan

### Root `README.md`

Purpose: sell the repo as a portfolio collection, not a raw project dump.

Recommended sections:

1. **Title:** `Workshops`
2. **Short pitch:** collection of hands-on technical workshops designed and delivered by Bruno Copa.
3. **Workshop index:** table with columns: Workshop, Topic, Audience, Status, Link.
4. **Featured workshop:** Energy Demand Forecasting.
5. **What this demonstrates:** curriculum design, applied ML, notebook-based teaching, data storytelling, production-minded tooling.
6. **Repo structure:** concise tree.
7. **Notes:** data files are excluded because they are large/local; notebooks are preserved as delivered.

Example tone:

> A curated collection of hands-on technical workshops covering applied machine learning, data engineering, and engineering enablement. Each workshop includes participant materials, solution notebooks, and delivery documentation where available.

### `energy-demand-forecasting/README.md`

Purpose: make the first workshop understandable in under 60 seconds.

Recommended sections:

1. **Title:** `Energy Demand Forecasting Workshop`
2. **Summary:** hands-on ML workshop for energy demand forecasting and anomaly detection.
3. **Audience:** consultants, data practitioners, engineers working with energy-sector data.
4. **Learning outcomes:** regression forecasting, LSTM forecasting, anomaly detection, LSTM autoencoders.
5. **Contents:** explain `student-materials/`, `solution-materials/`, `project/`, `docs/`, `archive/`.
6. **Suggested path:** start with student notebooks; compare with solutions; review docs for delivery context.
7. **Data note:** large `.hdf5` data is intentionally not committed; see `docs/data-sources-guide.md`.
8. **Portfolio note:** originally created for an energy-sector workshop; client/company-specific folder names have been generalized for portfolio presentation.

### `harness-engineering/README.md`

Purpose: placeholder for future workshop.

Suggested minimal content:

```markdown
# Harness Engineering Workshop

Placeholder for a future workshop focused on engineering harnesses, automation, testing workflows, and developer productivity systems.

Status: planned.
```

## Implementation Tasks

### Task 1: Create portfolio-level root README

**Description:** Add a polished root `README.md` that frames the repository as a workshop portfolio.

**Acceptance criteria:**
- [ ] `README.md` exists at repo root.
- [ ] It includes a workshop index with Energy Demand Forecasting and Harness Engineering.
- [ ] It explains the repository structure and data exclusion policy.

**Verification:**
- [ ] Read `README.md` in GitHub preview style.
- [ ] Confirm all relative links point to existing target folders after reorganization.

**Dependencies:** None.

**Estimated scope:** Small.

### Task 2: Create target workshop folders

**Description:** Create `energy-demand-forecasting/` and `harness-engineering/` at the root.

**Acceptance criteria:**
- [ ] `energy-demand-forecasting/` exists.
- [ ] `harness-engineering/README.md` exists as a placeholder.
- [ ] Folder naming is lowercase kebab-case.

**Verification:**
- [ ] `find . -maxdepth 2 -type d` shows the intended root folders.

**Dependencies:** None.

**Estimated scope:** Small.

### Task 3: Move student and solution notebooks into clear folders

**Description:** Move notebook files into `student-materials/` and `solution-materials/` with numbered, recruiter-readable names.

**Acceptance criteria:**
- [ ] Student notebooks are under `energy-demand-forecasting/student-materials/`.
- [ ] Solution notebooks are under `energy-demand-forecasting/solution-materials/`.
- [ ] Notebook names are sequential and descriptive.
- [ ] Notebook contents are unchanged.

**Verification:**
- [ ] Use `git diff --stat` to confirm moves/renames rather than content rewrites where possible.
- [ ] Optionally compare checksums before/after for notebooks.

**Dependencies:** Task 2.

**Estimated scope:** Medium.

### Task 4: Move project/runtime files into `project/`

**Description:** Move Python/project support files and assets out of the old `Shared/energy-demand/` folder into `energy-demand-forecasting/project/`.

**Acceptance criteria:**
- [ ] `main.py`, `pyproject.toml`, `uv.lock`, `.python-version`, and `assets/` live under `project/`.
- [ ] No local data files are tracked or moved into Git.
- [ ] `.venv/` remains ignored/untracked.

**Verification:**
- [ ] `git ls-files | grep data` does not show local data files.
- [ ] `git status --ignored --short` confirms local data/venv remain ignored.

**Dependencies:** Task 2.

**Estimated scope:** Small.

### Task 5: Move workshop documentation into `docs/`

**Description:** Move existing Markdown support files into `energy-demand-forecasting/docs/` with professional names.

**Acceptance criteria:**
- [ ] Curriculum, organizer guide, data guide, templates, start-here, and next-steps docs live under `docs/`.
- [ ] File contents are unchanged.
- [ ] Names no longer expose the original client/company as the organizing concept, except where content itself naturally references context.

**Verification:**
- [ ] `git diff --stat` shows renames/moves.
- [ ] Links from the workshop README point to the moved docs.

**Dependencies:** Task 2.

**Estimated scope:** Small.

### Task 6: Archive root-level duplicate/draft notebooks

**Description:** Move current root-level workshop notebooks into `energy-demand-forecasting/archive/` so they are preserved but not confused with student/solution materials.

**Acceptance criteria:**
- [ ] Root-level workshop notebooks no longer sit directly under the workshop folder.
- [ ] Archived notebooks are clearly labeled as preserved drafts/variants in the workshop README.
- [ ] Contents are unchanged.

**Verification:**
- [ ] `find energy-demand-forecasting -maxdepth 2 -type f -name '*.ipynb'` shows notebooks in expected folders.

**Dependencies:** Task 2.

**Estimated scope:** Small.

### Task 7: Add workshop-level README

**Description:** Add `energy-demand-forecasting/README.md` explaining the workshop, audience, learning outcomes, and folder layout.

**Acceptance criteria:**
- [ ] README explains student vs solution materials.
- [ ] README includes a data note and setup note.
- [ ] README avoids sounding like an internal/client delivery dump.
- [ ] README is concise and recruiter-friendly.

**Verification:**
- [ ] Read the README independently and confirm a recruiter can understand the project in under one minute.
- [ ] Verify all links resolve locally.

**Dependencies:** Tasks 3-6.

**Estimated scope:** Medium.

### Task 8: Update `.gitignore` for new folder paths

**Description:** Adjust ignore rules to match the new `energy-demand-forecasting/` structure.

**Acceptance criteria:**
- [ ] `.DS_Store` remains ignored.
- [ ] `.pi-subagents/` remains ignored.
- [ ] `energy-demand-forecasting/project/data/` or equivalent local data path is ignored.
- [ ] Any `.venv/` folders are ignored.

**Verification:**
- [ ] `git check-ignore -v` confirms local data and `.venv/` are ignored.
- [ ] `git ls-files` confirms no ignored local artifacts are tracked.

**Dependencies:** Tasks 2 and 4.

**Estimated scope:** Small.

### Task 9: Final quality pass

**Description:** Verify the repo looks clean and professional before committing/pushing.

**Acceptance criteria:**
- [ ] `git status --short` contains only intentional changes.
- [ ] No tracked `.DS_Store`, `.pi-subagents`, `.venv`, or large local data files.
- [ ] Folder names are clear and consistent.
- [ ] READMEs render well and links work.

**Verification commands:**

```bash
git status --short
git ls-files | grep -E '(^|/)\.DS_Store$|^\.pi-subagents/|(^|/)\.venv/|(^|/)data/.*\.(hdf5|csv|parquet)$' || true
find . -maxdepth 3 -type f -name 'README.md' -print
```

**Dependencies:** All previous tasks.

**Estimated scope:** Small.

## Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---:|---|
| Notebook JSON changes accidentally while renaming | Medium | Use `git mv`; do not open/save notebooks during the reorg. |
| Links inside existing docs may point to old names | Medium | Since content should not change, document known stale links in README or do a later link-fix pass with approval. |
| Existing root-level notebooks may duplicate solution notebooks | Low | Preserve them in `archive/` first; deduplicate only after checksum/content review. |
| Local data path changes may make ignored data less discoverable | Low | Add a clear data note in README and update `.gitignore` to ignore likely new data paths. |
| Recruiters may not understand original client context | Medium | Use READMEs to frame the work by topic, outcomes, and skills demonstrated. |

## Open Questions

1. Should the repo keep the default branch as `master`, or rename it to `main` for a more modern GitHub presentation?
2. Should old `Hochfrequenz` wording inside existing Markdown content be left untouched, or should we do a later content-polish pass? Current plan does **not** change file contents.
3. Are the root-level notebooks true duplicates, older drafts, or distinct deliverables? Current plan preserves them in `archive/`.
4. Should `Electrical House Heat Dataset.pdf` be moved into `energy-demand-forecasting/docs/` or kept at root? Recommendation: move to `energy-demand-forecasting/docs/electrical-house-heat-dataset.pdf` if it directly supports this workshop.

## Recommended Commit Strategy

1. Commit 1: `Add portfolio README and workshop placeholder`
2. Commit 2: `Reorganize energy demand workshop materials`
3. Commit 3: `Add energy demand workshop README`
4. Commit 4: `Update ignore rules and verify repository hygiene`

## Definition of Done

- [ ] Root repository presents as a multi-workshop portfolio.
- [ ] Energy Demand Forecasting workshop has clear student, solution, project, docs, and archive areas.
- [ ] Harness Engineering placeholder exists.
- [ ] No workshop contents are rewritten.
- [ ] No large/local data files or system artifacts are tracked.
- [ ] Changes are committed and pushed after review.
