# Source Library

> Tracks all external sources used in this repository. Every question maintains traceability back to its origin.

Sources are stored under `sources/{technology}/{source-type}/{source-name}/`.

---

## Sources

### GFE-001 — GreatFrontEnd: 100 React Interview Questions (2026)

| Field | Value |
|---|---|
| **Source ID** | GFE-001 |
| **Title** | 100+ React Interview Questions Straight from Ex-interviewers (2026) |
| **Publisher** | GreatFrontEnd |
| **URL** | https://www.greatfrontend.com/blog/100-react-interview-questions-straight-from-ex-interviewers |
| **Published** | May 20, 2026 |
| **Technology** | React |
| **Source type** | Website |
| **Questions extracted** | 110 |
| **Canonical IDs** | REACT-001 – REACT-110 |
| **Archive file** | [sources/react/website/greatfrontend/react-100-questions.md](../../sources/react/website/greatfrontend/react-100-questions.md) |

**Coverage map:**

| Source Section | Questions | Canonical IDs |
|---|---|---|
| React fundamentals | Q1–Q23 | REACT-001 – REACT-023 |
| React Hooks | Q24–Q34 | REACT-024 – REACT-034 |
| Advanced concepts | Q35–Q65 | REACT-035 – REACT-065 |
| React Router | Q66–Q79 | REACT-066 – REACT-079 |
| React Internationalization | Q80–Q86 | REACT-080 – REACT-086 |
| React Testing | Q87–Q100 | REACT-087 – REACT-100 |
| React 19 and modern React | Q101–Q110 | REACT-101 – REACT-110 |

---

### SDJ-001 — SudheerJ: React Interview Questions (GitHub)

| Field | Value |
|---|---|
| **Source ID** | SDJ-001 |
| **Title** | reactjs-interview-questions |
| **Author** | Sudheer Jonna (@SudheerJonna) |
| **GitHub** | https://github.com/sudheerj/reactjs-interview-questions |
| **Technology** | React |
| **Source type** | GitHub repository |
| **Total questions** | 408 |
| **New canonical IDs created** | REACT-111 – REACT-144 (34 new) |
| **Mapped to existing IDs** | ~85 questions (cross-referenced) |
| **SDJ-exclusive** | ~289 questions (niche/practical/legacy, see map) |
| **Archive file** | [sources/react/github/sudheerj-reactjs-interview-questions/question-map.md](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md) |

**Section breakdown:**

| SDJ Section | SDJ Questions | Mapped to Canonicals |
|---|---|---|
| Core React | 78 | REACT-001–023, 040, 052, 057, 059, 063 (and others) |
| React Router | 11 | REACT-066–079 |
| React Internationalization | 6 | REACT-081–086 |
| React Testing | 6 | REACT-088, 095, 100 |
| React Redux | 34 | REACT-111–135 (new) |
| React Native | 4 | REACT-136–139 (new) |
| Libraries & Integration | 13 | REACT-140–144 (new) |
| Miscellaneous | 167 | Scattered across all sections |
| Old Q&A | 89 | Legacy class component patterns |

---

## Adding a New Source

When adding questions from a new source, follow this process:

1. **Assign a Source ID** — format: `ABBREV-NNN` (e.g., `YT-001` for a YouTube source, `RD-001` for Roadmap.sh).
2. **Create source directory** — `sources/{technology}/{source-type}/{source-name}/`.
3. **Create a question-map.md** — map each source question to a canonical ID, or flag as source-exclusive.
4. **For new canonicals** — assign next available ID, write original answer in appropriate doc file.
5. **Update this Source Library** — add the new source entry and coverage map.
6. **Update [docs/react/master-index.md](../react/master-index.md)** — add new IDs.
7. **Update [README.md](../../README.md)** — add to Sources table.

---

## Planned Sources

| Source | Technology | Type | Status | Est. New Canonicals |
|---|---|---|---|---|
| GreatFrontEnd — 50 React Coding Questions | React | Website | Planned | ~20 |
| YouTube — React Interview Series | React | YouTube | Planned | ~30 |
| React Official Docs — Key Concepts | React | Website | Planned | ~20 |
| Roadmap.sh — React Developer Path | React | Website | Planned | ~15 |
| Next.js Official Docs | Next.js | Website | Planned | ~50 |

---

*← [Question Registry](./01-question-registry.md) | [Visual Assets →](./03-visual-assets.md)*
