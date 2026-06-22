# Source Archive — SudheerJ: React Interview Questions

> **Source:** https://github.com/sudheerj/reactjs-interview-questions
> **Author:** Sudheer Jonna (@SudheerJonna)
> **Repo:** `sudheerj/reactjs-interview-questions`
> **Questions extracted:** 408 (across 8 sections + Old Q&A)
> **Source type:** GitHub repository
>
> This file maps each SudheerJ question to its canonical ID in this repository.
> Original answers © Sudheer Jonna. Visit the source URL for full content.

---

## Overlap Summary

| Status | Count |
|---|---|
| Maps to existing canonical (GFE-sourced) | ~85 |
| New canonical entries created | 172 (REACT-111 – REACT-282) |
| Source-only / uncovered questions | 0 — every SDJ question has a canonical entry |
| **Total SDJ questions canonicalized** | **408** |

> All SDJ questions are now mapped to canonical entries. Previously "SDJ-exclusive" questions were added to the correct category files. See the [master index](../../../../docs/react/master-index.md) for the full list.

---

## Section 1 — Core React (78 questions)

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-001 | What is React? | [REACT-001](../../../../docs/react/01-fundamentals.md#react-001) | Mapped |
| SDJ-002 | What is the history behind React evolution? | — | SDJ-exclusive |
| SDJ-003 | What are the major features of React? | [REACT-001](../../../../docs/react/01-fundamentals.md#react-001) | Mapped (merged) |
| SDJ-004 | What is JSX? | [REACT-002](../../../../docs/react/01-fundamentals.md#react-002) | Mapped |
| SDJ-005 | What is the difference between an Element and a Component? | [REACT-004](../../../../docs/react/01-fundamentals.md#react-004) | Mapped |
| SDJ-006 | How to create components in React? | — | SDJ-exclusive |
| SDJ-007 | When to use a Class Component over a Function Component? | [REACT-010](../../../../docs/react/01-fundamentals.md#react-010) | Mapped |
| SDJ-008 | What are Pure Components? | [REACT-016](../../../../docs/react/01-fundamentals.md#react-016) | Mapped |
| SDJ-009 | What is state in React? | [REACT-008](../../../../docs/react/01-fundamentals.md#react-008) | Mapped |
| SDJ-010 | What are props in React? | [REACT-008](../../../../docs/react/01-fundamentals.md#react-008) | Mapped |
| SDJ-011 | What is the difference between state and props? | [REACT-008](../../../../docs/react/01-fundamentals.md#react-008) | Mapped |
| SDJ-012 | What is the difference between HTML and React event handling? | [REACT-052](../../../../docs/react/03-advanced.md#react-052) | Mapped |
| SDJ-013 | What are synthetic events in React? | [REACT-052](../../../../docs/react/03-advanced.md#react-052) | Mapped |
| SDJ-014 | What are inline conditional expressions? | — | SDJ-exclusive |
| SDJ-015 | What is "key" prop and what is the benefit of using it? | [REACT-006](../../../../docs/react/01-fundamentals.md#react-006) | Mapped |
| SDJ-016 | What is Virtual DOM? | [REACT-003](../../../../docs/react/01-fundamentals.md#react-003) | Mapped |
| SDJ-017 | How Virtual DOM works? | [REACT-003](../../../../docs/react/01-fundamentals.md#react-003) | Mapped |
| SDJ-018 | Difference between Shadow DOM and Virtual DOM? | [REACT-013](../../../../docs/react/01-fundamentals.md#react-013) | Mapped |
| SDJ-019 | What is React Fiber? | [REACT-011](../../../../docs/react/01-fundamentals.md#react-011) | Mapped |
| SDJ-020 | What is the main goal of React Fiber? | [REACT-011](../../../../docs/react/01-fundamentals.md#react-011) | Mapped (merged) |
| SDJ-021 | What are controlled components? | [REACT-014](../../../../docs/react/01-fundamentals.md#react-014) | Mapped |
| SDJ-022 | What are uncontrolled components? | [REACT-014](../../../../docs/react/01-fundamentals.md#react-014) | Mapped |
| SDJ-023 | Difference between createElement and cloneElement? | [REACT-017](../../../../docs/react/01-fundamentals.md#react-017) | Mapped |
| SDJ-024 | What is Lifting State Up in React? | [REACT-015](../../../../docs/react/01-fundamentals.md#react-015) | Mapped |
| SDJ-025 | What are Higher-Order Components? | [REACT-059](../../../../docs/react/03-advanced.md#react-059) | Mapped |
| SDJ-026 | What is children prop? | — | SDJ-exclusive |
| SDJ-027 | How to write comments in React? | — | SDJ-exclusive |
| SDJ-028 | What is reconciliation? | [REACT-012](../../../../docs/react/01-fundamentals.md#react-012) | Mapped |
| SDJ-029 | Does the lazy function support named exports? | — | SDJ-exclusive |
| SDJ-030 | Why React uses className over class attribute? | [REACT-002](../../../../docs/react/01-fundamentals.md#react-002) | Mapped (JSX rules) |
| SDJ-031 | What are fragments? | [REACT-005](../../../../docs/react/01-fundamentals.md#react-005) | Mapped |
| SDJ-032 | Why fragments are better than container divs? | [REACT-005](../../../../docs/react/01-fundamentals.md#react-005) | Mapped |
| SDJ-033 | What are portals in React? | [REACT-040](../../../../docs/react/03-advanced.md#react-040) | Mapped |
| SDJ-034 | What are stateless components? | [REACT-019](../../../../docs/react/01-fundamentals.md#react-019) | Mapped |
| SDJ-035 | What are stateful components? | [REACT-020](../../../../docs/react/01-fundamentals.md#react-020) | Mapped |
| SDJ-036 | How to apply validation on props in React? | [REACT-018](../../../../docs/react/01-fundamentals.md#react-018) | Mapped |
| SDJ-037 | What are the advantages of React? | [REACT-001](../../../../docs/react/01-fundamentals.md#react-001) | Mapped |
| SDJ-038 | What are the limitations of React? | — | SDJ-exclusive |
| SDJ-039 | Recommended ways for static type checking? | [REACT-021](../../../../docs/react/01-fundamentals.md#react-021) | Mapped |
| SDJ-040 | What is the use of react-dom package? | — | SDJ-exclusive |
| SDJ-041 | What is ReactDOMServer? | [REACT-057](../../../../docs/react/03-advanced.md#react-057) | Mapped (SSR) |
| SDJ-042 | How to use innerHTML in React? | — | SDJ-exclusive |
| SDJ-043 | How to use styles in React? | — | SDJ-exclusive |
| SDJ-044 | How events are different in React? | [REACT-052](../../../../docs/react/03-advanced.md#react-052) | Mapped |
| SDJ-045 | What is the impact of indexes as keys? | [REACT-007](../../../../docs/react/01-fundamentals.md#react-007) | Mapped |
| SDJ-046 | How do you conditionally render components? | — | SDJ-exclusive |
| SDJ-047 | Why be careful when spreading props on DOM elements? | — | SDJ-exclusive |
| SDJ-048 | How do you memoize a component? | [REACT-016](../../../../docs/react/01-fundamentals.md#react-016) | Mapped |
| SDJ-049 | How you implement Server Side Rendering? | [REACT-057](../../../../docs/react/03-advanced.md#react-057) | Mapped |
| SDJ-050 | How to enable production mode in React? | — | SDJ-exclusive |
| SDJ-051 | Do Hooks replace render props and HOCs? | [REACT-061](../../../../docs/react/03-advanced.md#react-061) | Mapped |
| SDJ-052 | What is a switching component? | — | SDJ-exclusive |
| SDJ-053 | What are React Mixins? | — | SDJ-exclusive (legacy) |
| SDJ-054 | What are the Pointer Events supported in React? | — | SDJ-exclusive |
| SDJ-055 | Why should component names start with capital letter? | [REACT-002](../../../../docs/react/01-fundamentals.md#react-002) | Mapped (JSX) |
| SDJ-056 | Are custom DOM attributes supported in React v16? | — | SDJ-exclusive |
| SDJ-057 | How to loop inside JSX? | — | SDJ-exclusive |
| SDJ-058 | How do you access props in attribute quotes? | — | SDJ-exclusive |
| SDJ-059 | What is React proptype array with shape? | [REACT-018](../../../../docs/react/01-fundamentals.md#react-018) | Mapped |
| SDJ-060 | How to conditionally apply class attributes? | — | SDJ-exclusive |
| SDJ-061 | Difference between React and ReactDOM? | — | SDJ-exclusive |
| SDJ-062 | Why ReactDOM is separated from React? | — | SDJ-exclusive |
| SDJ-063 | How to use React label element? | — | SDJ-exclusive |
| SDJ-064 | How to combine multiple inline style objects? | — | SDJ-exclusive |
| SDJ-065 | How to re-render the view when browser is resized? | [REACT-063](../../../../docs/react/03-advanced.md#react-063) | Mapped |
| SDJ-066 | How to pretty print JSON with React? | — | SDJ-exclusive |
| SDJ-067 | Why can't you update props in React? | [REACT-008](../../../../docs/react/01-fundamentals.md#react-008) | Mapped |
| SDJ-068 | How to focus an input element on page load? | [REACT-027](../../../../docs/react/02-hooks.md#react-027) | Mapped (useRef) |
| SDJ-069 | How to find React version at runtime? | — | SDJ-exclusive |
| SDJ-070 | How to add Google Analytics for React Router? | — | SDJ-exclusive |
| SDJ-071 | How to apply vendor prefixes to inline styles? | — | SDJ-exclusive |
| SDJ-072 | How to import and export components using ES6? | — | SDJ-exclusive |
| SDJ-073 | What are the exceptions on React component naming? | — | SDJ-exclusive |
| SDJ-074 | Is it possible to use async/await in plain React? | — | SDJ-exclusive |
| SDJ-075 | What are the common folder structures for React? | — | SDJ-exclusive |
| SDJ-076 | What are the popular packages for animation? | — | SDJ-exclusive |
| SDJ-077 | What is the benefit of styles modules? | — | SDJ-exclusive |
| SDJ-078 | What are the popular React-specific linters? | — | SDJ-exclusive |

---

## Section 2 — React Router (11 questions)

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-079 | What is React Router? | [REACT-066](../../../../docs/react/04-routing.md#react-066) | Mapped |
| SDJ-080 | How React Router is different from history library? | [REACT-070](../../../../docs/react/04-routing.md#react-070) | Mapped |
| SDJ-081 | What are the `<Router>` components of React Router v6? | [REACT-071](../../../../docs/react/04-routing.md#react-071) | Mapped |
| SDJ-082 | Purpose of push() and replace() methods of history? | [REACT-072](../../../../docs/react/04-routing.md#react-072) | Mapped |
| SDJ-083 | How to programmatically navigate using React Router? | [REACT-073](../../../../docs/react/04-routing.md#react-073) | Mapped |
| SDJ-084 | How to get query parameters in React Router? | [REACT-077](../../../../docs/react/04-routing.md#react-077) | Mapped |
| SDJ-085 | Why "Router may have only one child element" warning? | — | SDJ-exclusive |
| SDJ-086 | How to pass params to history.push method? | [REACT-073](../../../../docs/react/04-routing.md#react-073) | Mapped |
| SDJ-087 | How to implement default or NotFound page? | [REACT-076](../../../../docs/react/04-routing.md#react-076) | Mapped |
| SDJ-088 | How to get history on React Router? | [REACT-070](../../../../docs/react/04-routing.md#react-070) | Mapped |
| SDJ-089 | How to perform automatic redirect after login? | [REACT-078](../../../../docs/react/04-routing.md#react-078) | Mapped |

---

## Section 3 — React Internationalization (6 questions)

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-090 | What is React Intl? | [REACT-081](../../../../docs/react/05-i18n.md#react-081) | Mapped |
| SDJ-091 | What are the main features of React Intl? | [REACT-082](../../../../docs/react/05-i18n.md#react-082) | Mapped |
| SDJ-092 | Two ways of formatting in React Intl? | [REACT-083](../../../../docs/react/05-i18n.md#react-083) | Mapped |
| SDJ-093 | How to use FormattedMessage as placeholder? | [REACT-084](../../../../docs/react/05-i18n.md#react-084) | Mapped |
| SDJ-094 | How to access current locale with React Intl? | [REACT-085](../../../../docs/react/05-i18n.md#react-085) | Mapped |
| SDJ-095 | How to format date using React Intl? | [REACT-086](../../../../docs/react/05-i18n.md#react-086) | Mapped |

---

## Section 4 — React Testing (6 questions)

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-096 | What is Shallow Renderer in React testing? | [REACT-095](../../../../docs/react/06-testing.md#react-095) | Mapped |
| SDJ-097 | What is TestRenderer package in React? | [REACT-100](../../../../docs/react/06-testing.md#react-100) | Mapped |
| SDJ-098 | What is the purpose of ReactTestUtils package? | — | SDJ-exclusive |
| SDJ-099 | What is Jest? | [REACT-088](../../../../docs/react/06-testing.md#react-088) | Mapped |
| SDJ-100 | What are the advantages of Jest over Jasmine? | [REACT-088](../../../../docs/react/06-testing.md#react-088) | Mapped |
| SDJ-101 | Give a simple example of Jest test case | [REACT-088](../../../../docs/react/06-testing.md#react-088) | Mapped |

---

## Section 5 — React Redux (34 questions)

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-102 | What is Flux? | [REACT-044](../../../../docs/react/03-advanced.md#react-044) | Mapped |
| SDJ-103 | What is Redux? | [REACT-111](../../../../docs/react/08-redux.md#react-111) | New |
| SDJ-104 | What are the core principles of Redux? | [REACT-112](../../../../docs/react/08-redux.md#react-112) | New |
| SDJ-105 | Downsides of Redux compared to Flux? | [REACT-113](../../../../docs/react/08-redux.md#react-113) | New |
| SDJ-106 | Difference between mapStateToProps() and mapDispatchToProps()? | [REACT-114](../../../../docs/react/08-redux.md#react-114) | New |
| SDJ-107 | Can I dispatch an action in reducer? | [REACT-115](../../../../docs/react/08-redux.md#react-115) | New |
| SDJ-108 | How to access Redux store outside a component? | [REACT-116](../../../../docs/react/08-redux.md#react-116) | New |
| SDJ-109 | What are the drawbacks of MVW pattern? | [REACT-113](../../../../docs/react/08-redux.md#react-113) | Mapped |
| SDJ-110 | Are there any similarities between Redux and RxJS? | — | SDJ-exclusive |
| SDJ-111 | How to reset state in Redux? | [REACT-117](../../../../docs/react/08-redux.md#react-117) | New |
| SDJ-112 | Difference between React context and React Redux? | [REACT-118](../../../../docs/react/08-redux.md#react-118) | New |
| SDJ-113 | Why are Redux state functions called reducers? | [REACT-119](../../../../docs/react/08-redux.md#react-119) | New |
| SDJ-114 | How to make AJAX request in Redux? | [REACT-120](../../../../docs/react/08-redux.md#react-120) | New |
| SDJ-115 | Should I keep all component's state in Redux store? | [REACT-121](../../../../docs/react/08-redux.md#react-121) | New |
| SDJ-116 | What is the proper way to access Redux store? | [REACT-116](../../../../docs/react/08-redux.md#react-116) | Mapped |
| SDJ-117 | Difference between component and container in React Redux? | [REACT-122](../../../../docs/react/08-redux.md#react-122) | New |
| SDJ-118 | What is the purpose of constants in Redux? | [REACT-123](../../../../docs/react/08-redux.md#react-123) | New |
| SDJ-119 | Different ways to write mapDispatchToProps()? | [REACT-114](../../../../docs/react/08-redux.md#react-114) | Mapped |
| SDJ-120 | What is the use of ownProps parameter? | [REACT-114](../../../../docs/react/08-redux.md#react-114) | Mapped |
| SDJ-121 | How to structure Redux top level directories? | [REACT-124](../../../../docs/react/08-redux.md#react-124) | New |
| SDJ-122 | What is redux-saga? | [REACT-125](../../../../docs/react/08-redux.md#react-125) | New |
| SDJ-123 | What is the mental model of redux-saga? | [REACT-125](../../../../docs/react/08-redux.md#react-125) | Mapped |
| SDJ-124 | Differences between call() and put() in redux-saga? | [REACT-126](../../../../docs/react/08-redux.md#react-126) | New |
| SDJ-125 | What is Redux Thunk? | [REACT-127](../../../../docs/react/08-redux.md#react-127) | New |
| SDJ-126 | Differences between redux-saga and redux-thunk? | [REACT-128](../../../../docs/react/08-redux.md#react-128) | New |
| SDJ-127 | What is Redux DevTools? | [REACT-129](../../../../docs/react/08-redux.md#react-129) | New |
| SDJ-128 | What are the features of Redux DevTools? | [REACT-129](../../../../docs/react/08-redux.md#react-129) | Mapped |
| SDJ-129 | What are Redux selectors and why use them? | [REACT-130](../../../../docs/react/08-redux.md#react-130) | New |
| SDJ-130 | What is Redux Form? | [REACT-131](../../../../docs/react/08-redux.md#react-131) | New |
| SDJ-131 | What are the main features of Redux Form? | [REACT-131](../../../../docs/react/08-redux.md#react-131) | Mapped |
| SDJ-132 | How to add multiple middlewares to Redux? | [REACT-132](../../../../docs/react/08-redux.md#react-132) | New |
| SDJ-133 | How to set initial state in Redux? | [REACT-133](../../../../docs/react/08-redux.md#react-133) | New |
| SDJ-134 | How Relay is different from Redux? | [REACT-134](../../../../docs/react/08-redux.md#react-134) | New |
| SDJ-135 | What is an action in Redux? | [REACT-135](../../../../docs/react/08-redux.md#react-135) | New |

---

## Section 6 — React Native (4 questions)

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-136 | Difference between React Native and React? | [REACT-136](../../../../docs/react/09-react-native.md#react-136) | New |
| SDJ-137 | How to test React Native apps? | [REACT-137](../../../../docs/react/09-react-native.md#react-137) | New |
| SDJ-138 | How to do logging in React Native? | [REACT-138](../../../../docs/react/09-react-native.md#react-138) | New |
| SDJ-139 | How to debug your React Native? | [REACT-139](../../../../docs/react/09-react-native.md#react-139) | New |

---

## Section 7 — React Libraries & Integration (13 questions)

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-140 | What is reselect and how it works? | [REACT-140](../../../../docs/react/10-libraries.md#react-140) | New |
| SDJ-141 | What is Flow? | [REACT-141](../../../../docs/react/10-libraries.md#react-141) | New |
| SDJ-142 | Difference between Flow and PropTypes? | [REACT-142](../../../../docs/react/10-libraries.md#react-142) | New |
| SDJ-143 | How to use Font Awesome icons in React? | — | SDJ-exclusive |
| SDJ-144 | What is React Dev Tools? | [REACT-143](../../../../docs/react/10-libraries.md#react-143) | New |
| SDJ-145 | Why is DevTools not loading in Chrome for local files? | — | SDJ-exclusive |
| SDJ-146 | How to use Polymer in React? | — | SDJ-exclusive |
| SDJ-147 | Advantages of React over Vue.js? | — | SDJ-exclusive |
| SDJ-148 | Difference between React and Angular? | — | SDJ-exclusive |
| SDJ-149 | Why React tab is not showing up in DevTools? | — | SDJ-exclusive |
| SDJ-150 | What are Styled Components? | [REACT-144](../../../../docs/react/10-libraries.md#react-144) | New |
| SDJ-151 | Give an example of Styled Components? | [REACT-144](../../../../docs/react/10-libraries.md#react-144) | Mapped |
| SDJ-152 | What is Relay? | [REACT-134](../../../../docs/react/08-redux.md#react-134) | Mapped |

---

## Section 8 — Miscellaneous (selected key questions)

> The Miscellaneous section has 167 questions. Below are the ones that map to new or existing canonicals. SDJ-exclusive practical questions are noted but not individually listed.

| SDJ # | Question | Canonical ID | Status |
|---|---|---|---|
| SDJ-168 | What are hooks? | [REACT-023](../../../../docs/react/01-fundamentals.md#react-023) | Mapped |
| SDJ-169 | What rules need to be followed for hooks? | [REACT-024](../../../../docs/react/02-hooks.md#react-024) | Mapped |
| SDJ-181 | What is code-splitting? | [REACT-042](../../../../docs/react/03-advanced.md#react-042) | Mapped |
| SDJ-185 | What is NextJS and major features of it? | [docs/nextjs/README.md](../../../../docs/nextjs/README.md) | Mapped |
| SDJ-199 | What is suspense component? | [REACT-038](../../../../docs/react/03-advanced.md#react-038) | Mapped |
| SDJ-200 | What is route based code splitting? | [REACT-042](../../../../docs/react/03-advanced.md#react-042) | Mapped |
| SDJ-202 | What is diffing algorithm? | [REACT-012](../../../../docs/react/01-fundamentals.md#react-012) | Mapped |
| SDJ-217 | What is useEffect hook? How to fetch data with React Hooks? | [REACT-026](../../../../docs/react/02-hooks.md#react-026) | Mapped |
| SDJ-230 | What is Concurrent Rendering? | [REACT-054](../../../../docs/react/03-advanced.md#react-054) | Mapped |
| SDJ-239 | What are React Server components? | [REACT-106](../../../../docs/react/07-react19.md#react-106) | Mapped |
| SDJ-240 | What is prop drilling? | [REACT-050](../../../../docs/react/03-advanced.md#react-050) | Mapped |
| SDJ-241 | Difference between useState and useRef hook? | [REACT-027](../../../../docs/react/02-hooks.md#react-027) | Mapped |
| SDJ-243 | Differences between useEffect and useLayoutEffect? | [REACT-025](../../../../docs/react/02-hooks.md#react-025) | Mapped |
| SDJ-244 | Differences between Functional and Class Components? | [REACT-009](../../../../docs/react/01-fundamentals.md#react-009) | Mapped |
| SDJ-245 | What is strict mode in React? | [REACT-041](../../../../docs/react/03-advanced.md#react-041) | Mapped |
| SDJ-253 | How does React batch multiple state updates? | [REACT-049](../../../../docs/react/03-advanced.md#react-049) | Mapped |
| SDJ-255 | What is React hydration? | [REACT-039](../../../../docs/react/03-advanced.md#react-039) | Mapped |
| SDJ-266 | How is useReducer Different from useState? | [REACT-031](../../../../docs/react/02-hooks.md#react-031) | Mapped |
| SDJ-267 | What is useContext? | [REACT-033](../../../../docs/react/02-hooks.md#react-033) | Mapped |
| SDJ-271 | Can you describe the useMemo() Hook? | [REACT-030](../../../../docs/react/02-hooks.md#react-030) | Mapped |
| SDJ-279 | What is useReducer? Why do you use useReducer? | [REACT-031](../../../../docs/react/02-hooks.md#react-031) | Mapped |
| SDJ-296 | How Do You Use useRef to Access a DOM Element? | [REACT-027](../../../../docs/react/02-hooks.md#react-027) | Mapped |
| SDJ-304 | How is useMemo different from useCallback? | [REACT-030](../../../../docs/react/02-hooks.md#react-030) | Mapped |
| SDJ-306 | What is useCallback and why is it used? | [REACT-029](../../../../docs/react/02-hooks.md#react-029) | Mapped |
| SDJ-307 | What are Custom React Hooks? | [REACT-034](../../../../docs/react/02-hooks.md#react-034) | Mapped |
| SDJ-310 | What is the useId hook? | [REACT-032](../../../../docs/react/02-hooks.md#react-032) | Mapped |
| SDJ-311 | What is the useDeferredValue hook? | [REACT-109](../../../../docs/react/07-react19.md#react-109) | Mapped |
| SDJ-312 | What is useTransition vs useDeferredValue? | [REACT-109](../../../../docs/react/07-react19.md#react-109) | Mapped |

---

*← [Source Library](../../../../docs/reference/02-source-library.md) | [Master Index](../../../../docs/reference/00-master-index.md)*
