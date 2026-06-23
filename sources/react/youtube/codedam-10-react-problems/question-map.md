# Source Archive — CodeDam: 10 React.js Interview-Level Problems (YouTube)

> **Source:** https://www.youtube.com/watch?v=qUx6ajDs5UM
> **Channel:** CodeDam
> **Technology:** React
> **Source type:** YouTube
> **Format:** Live coding challenge walkthrough — 11 problems solved in real time on codedam.com/problem-list/react
> **Source ID:** CDM-001

---

## Overlap Summary

| Status | Count |
|---|---|
| Mapped to existing canonical | 7 |
| New canonical created | 4 |
| Source-only | 0 |
| **Total** | **11** |

> All 11 problems are canonicalized. 4 new entries cover Immer, progress bar pattern, localStorage persistence, and form validation.

---

## Problem List

| CDM # | Problem / Topic | Timestamp | Canonical ID | Status |
|---|---|---|---|---|
| 1 | Progress bar component — width capping, style-as-object, `Math.min(percentage, 100)` | [00:00:47](https://youtu.be/qUx6ajDs5UM?t=47) | [REACT-286](../../../../docs/react/03-advanced.md#react-286) | New |
| 2 | Select all checkboxes — `checked` state, toggle all | [00:01:43](https://youtu.be/qUx6ajDs5UM?t=103) | [REACT-285](../../../../docs/react/10-libraries.md#react-285) | New |
| 3 | Using Immer for nested state mutations — `produce()`, direct mutation syntax | [00:02:13](https://youtu.be/qUx6ajDs5UM?t=133) | [REACT-285](../../../../docs/react/10-libraries.md#react-285) | New (merged) |
| 4 | Styled components button — mandatory `styled.button` usage | [00:03:18](https://youtu.be/qUx6ajDs5UM?t=198) | [REACT-144](../../../../docs/react/10-libraries.md#react-144) | Mapped |
| 5 | Hover counter — `onMouseEnter` vs `onMouseOver`, state increment | [00:04:30](https://youtu.be/qUx6ajDs5UM?t=270) | [REACT-024](../../../../docs/react/02-hooks.md#react-024) | Mapped |
| 6 | Fix bugs in a React component — JSX syntax, missing return, class vs className | [00:05:06](https://youtu.be/qUx6ajDs5UM?t=306) | [REACT-002](../../../../docs/react/01-fundamentals.md#react-002) | Mapped |
| 7 | Tab bar with active state — single active tab, conditional class, prop drilling active state | [00:05:50](https://youtu.be/qUx6ajDs5UM?t=350) | [REACT-008](../../../../docs/react/01-fundamentals.md#react-008) | Mapped |
| 8 | Local storage counter — persisting count across refresh, `localStorage.getItem/setItem`, `useEffect` sync | [00:06:50](https://youtu.be/qUx6ajDs5UM?t=410) | [REACT-287](../../../../docs/react/02-hooks.md#react-287) | New |
| 9 | Console log on button click — `onClick` handler | [00:08:31](https://youtu.be/qUx6ajDs5UM?t=511) | [REACT-024](../../../../docs/react/02-hooks.md#react-024) | Mapped |
| 10 | Sign-up form validation — firstName, lastName, email regex, password length, confirm password match, Immer for error state | [00:09:04](https://youtu.be/qUx6ajDs5UM?t=544) | [REACT-288](../../../../docs/react/03-advanced.md#react-288) | New |
| 11 | Fix class-based component — `count + 1` vs `count++`, `setState` pattern | [00:15:42](https://youtu.be/qUx6ajDs5UM?t=942) | [REACT-009](../../../../docs/react/01-fundamentals.md#react-009) | Mapped |
| 12 | `useTheme` custom hook — `useState` for theme toggle, returning `{ theme, toggleTheme }`, `useMemo` for return value | [00:16:15](https://youtu.be/qUx6ajDs5UM?t=975) | [REACT-034](../../../../docs/react/02-hooks.md#react-034) | Mapped |

---

## Key Concepts Demonstrated

- **Immer** (`produce` / `draft` pattern) — problems 2, 3, 10
- **`Math.min` / `Math.max` for value clamping** — problem 1
- **`onMouseEnter` vs `onMouseOver`** — problem 5 (onMouseOver fires repeatedly on children; onMouseEnter fires once)
- **Local storage + `useEffect`** — problem 8 (read from storage as initial state, sync on every count change)
- **Form validation with Immer error state** — problem 10 (email regex, password length, confirm match, reset errors on valid submit)
- **Custom hook return value memoization** — problem 12 (`useMemo` for stable object reference)

---

*Source folder: `sources/react/youtube/codedam-10-react-problems/`*
*Platform: [codedam.com/problem-list/react](https://codedam.com/problem-list/react)*
