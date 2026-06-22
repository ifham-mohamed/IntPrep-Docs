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
| **New canonical IDs created** | REACT-111 – REACT-282 (172 new) |
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
| Libraries & Integration | 13 | REACT-140–144 (new) + REACT-262–267 (new extended) |
| Hooks (miscellaneous/extended) | — | REACT-196–225 (new) |
| Advanced (miscellaneous/extended) | — | REACT-226–255 (new) |
| Old Q&A | 89 | REACT-268–282 (new — legacy class components) |

---

### GFE-NJS — GreatFrontEnd: Next.js Interview Questions for Freshers (2026)

| Field | Value |
|---|---|
| **Source ID** | GFE-NJS |
| **Title** | Next.js Interview Questions for Freshers: 35 Questions with Answers |
| **Publisher** | GreatFrontEnd |
| **URL** | https://www.greatfrontend.com/blog/next-js-interview-questions-for-freshers |
| **Published** | Jun 9, 2026 |
| **Technology** | Next.js |
| **Source type** | Website |
| **Questions extracted** | 35 |
| **Mapped to existing IDs** | 0 (Next.js domain was empty) |
| **New canonical IDs created** | NEXT-001 – NEXT-035 (35 new) |
| **All questions canonicalized** | Yes — all 35 questions have canonical entries |
| **Archive file** | [sources/nextjs/website/greatfrontend/question-map.md](../../sources/nextjs/website/greatfrontend/question-map.md) |

**Coverage map:**

| Source Section | Questions | Canonical IDs |
|---|---|---|
| Framework & App Router fundamentals | Q1–Q10 | NEXT-001 – NEXT-010 |
| Server Components & Client Components | Q11–Q14 | NEXT-011 – NEXT-014 |
| Rendering models (SSR, SSG, CSR, ISR) | Q15–Q18 | NEXT-015 – NEXT-018 |
| Data fetching & caching | Q19–Q22 | NEXT-019 – NEXT-022 |
| Navigation | Q23–Q25 | NEXT-023 – NEXT-025 |
| Route Handlers & Server Actions | Q26–Q28 | NEXT-026 – NEXT-028 |
| SEO, assets & metadata | Q29–Q31 | NEXT-029 – NEXT-031 |
| Infrastructure (Proxy, env, auth, deploy) | Q32–Q35 | NEXT-032 – NEXT-035 |

---

## Adding a New Source

When adding questions from a new source, follow this process:

**Rule: Every question in a source must become (or link to) a canonical entry.** There is no "source-only" bucket — all questions get a REACT-NNN or NEXT-NNN ID.

1. **Assign a Source ID** — format: `ABBREV-NNN` (e.g., `YT-001` for YouTube, `RD-001` for Roadmap.sh).
2. **Create source directory** — `sources/{technology}/{source-type}/{source-name}/`.
3. **Create a question-map.md** — map EVERY source question to either an existing canonical ID or a new one.
4. **For new canonicals** — assign next available ID (React: REACT-283 | Next.js: NEXT-036), write original answer in the correct doc file.
5. **Update this Source Library** — add the new source entry and coverage map.
6. **Update `docs/nextjs/README.md` or `docs/react/master-index.md`** — add new ID rows.
7. **Update [docs/reference/00-master-index.md](./00-master-index.md)** — add new rows.
8. **Update [docs/reference/01-question-registry.md](./01-question-registry.md)** — add new registry rows.
9. **Update [README.md](../../README.md)** — update Sources table and question count.

---

## Planned Sources

| Source | Technology | Type | Status | Est. New Canonicals |
|---|---|---|---|---|
| GreatFrontEnd — 50 React Coding Questions | React | Website | Planned | ~20 |
| YouTube — React Interview Series | React | YouTube | Planned | ~30 |
| React Official Docs — Key Concepts | React | Website | Planned | ~20 |
| Roadmap.sh — React Developer Path | React | Website | Planned | ~15 |
| Next.js Official Docs | Next.js | Website | Planned | ~50 |
| GreatFrontEnd — Next.js Interview Questions (intermediate) | Next.js | Website | Planned | ~30 |

---

*← [Question Registry](./01-question-registry.md) | [Visual Assets →](./03-visual-assets.md)*
