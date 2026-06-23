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
| **New canonical IDs created** | REACT-111 – REACT-283 (173 new) |
| **Mapped to existing IDs** | ~85 questions (cross-referenced to GFE canonicals) |
| **All questions canonicalized** | Yes — every SDJ question maps to a canonical entry |
| **Archive file** | [sources/react/github/sudheerj-reactjs-interview-questions/question-map.md](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md) |

**Section breakdown:**

| SDJ Section | SDJ Questions | Canonical IDs |
|---|---|---|
| Core React | 78 | REACT-001–023 (mapped) + REACT-145–195 (new extended) |
| React Router | 11 | REACT-066–079 (mapped) + REACT-256–257 (new) |
| React Internationalization | 6 | REACT-081–086 (mapped) |
| React Testing | 6 | REACT-088, 095, 100 (mapped) + REACT-258 (new) |
| React Redux | 34 | REACT-111–135 (new) + REACT-259–261 (new extended) |
| React Native | 4 | REACT-136–139 (new) |
| Libraries & Integration | 13 | REACT-140–144 (new) + REACT-262–267, 283 (new extended) |
| Hooks (miscellaneous/extended) | — | REACT-196–225 (new) |
| Advanced (miscellaneous/extended) | — | REACT-226–255 (new) |
| Old Q&A | 89 | REACT-268–282 (new — legacy class components) |

---

### GFE-002 — GreatFrontEnd: Top React Interview Questions (GitHub)

| Field | Value |
|---|---|
| **Source ID** | GFE-002 |
| **Title** | top-reactjs-interview-questions |
| **Publisher** | GreatFrontEnd |
| **GitHub** | https://github.com/greatfrontend/top-reactjs-interview-questions |
| **Technology** | React |
| **Source type** | GitHub repository |
| **Total questions** | 50 |
| **New canonical IDs created** | REACT-284 (1 new) |
| **Mapped to existing IDs** | 49 questions (cross-referenced to GFE-001 / SDJ canonicals) |
| **All questions canonicalized** | Yes — every GFE-002 question maps to a canonical entry |
| **Archive file** | [sources/react/github/greatfrontend-top-reactjs-interview-questions/question-map.md](../../sources/react/github/greatfrontend-top-reactjs-interview-questions/question-map.md) |

**Coverage map:**

| GFE-002 Section | Questions | Canonical IDs |
|---|---|---|
| Basics | Q1–Q8 | REACT-001, 002, 004, 006, 007, 008, 014, 046 |
| Hooks | Q9–Q18 | REACT-023–032 |
| Intermediate | Q19–Q25 | REACT-005, 022, 035, 036, 037, 087, 189 |
| Advanced | Q26–Q32 | REACT-038–043, 080, 284 (new) |
| Patterns | Q33–Q44 | REACT-044–048, 057–065 |
| Internals | Q45–Q50 | REACT-003, 011, 012, 038, 049 |

---

## Adding a New Source

When adding questions from a new source, follow this process:

**Rule: Every question in a source must become (or link to) a canonical entry.** There is no "source-only" bucket — all questions get a REACT-NNN ID.

1. **Assign a Source ID** — format: `ABBREV-NNN` (e.g., `YT-001` for YouTube, `RD-001` for Roadmap.sh).
2. **Create source directory** — `sources/{technology}/{source-type}/{source-name}/`.
3. **Create a question-map.md** — map EVERY source question to either an existing canonical ID or a new one.
4. **For new canonicals** — assign next available ID (currently REACT-285), write original answer in the correct doc file.
5. **Update this Source Library** — add the new source entry and coverage map.
6. **Update [docs/react/master-index.md](../react/master-index.md)** — add new ID rows.
7. **Update [docs/reference/00-master-index.md](./00-master-index.md)** — add new rows.
8. **Update [docs/reference/01-question-registry.md](./01-question-registry.md)** — add new registry rows.
9. **Update [README.md](../../README.md)** — add to Sources table and update count.

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
