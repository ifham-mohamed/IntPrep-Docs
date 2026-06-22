# React Native

> Canonical answers for REACT-136 to REACT-139.
> Sources: [SudheerJ](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-136

### What is the difference between React Native and React?

React is a JavaScript library for building web UIs. React Native is a framework for building native mobile applications (iOS and Android) using React's component model.

| Aspect | React (Web) | React Native |
|---|---|---|
| Output | HTML DOM elements | Native UI components |
| Primitives | `<div>`, `<span>`, `<img>` | `<View>`, `<Text>`, `<Image>` |
| Styling | CSS (stylesheets, CSS-in-JS) | JavaScript `StyleSheet` object, Flexbox only |
| Navigation | React Router, browser history | React Navigation, native stack navigators |
| Platform APIs | Browser APIs (fetch, localStorage) | Native modules (Camera, GPS, Bluetooth) |
| Renderer | `react-dom` | `react-native` renderer |
| Runtime | Browser (V8, SpiderMonkey) | JavaScriptCore (iOS), Hermes (Android) |

**What they share:** React's programming model (components, JSX, hooks, state, props, reconciliation) is identical. A React developer can learn React Native quickly because the mental model transfers. Business logic and non-UI hooks are often fully reusable between web and native.

With **Expo**, React Native development is significantly simpler — it provides a managed workflow, OTA updates, and access to native APIs without needing Xcode or Android Studio for most tasks.

**Related:** [REACT-001 — What is React](./01-fundamentals.md#react-001) | [REACT-057 — SSR](./03-advanced.md#react-057)

**Source:** [SudheerJ SDJ-136](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-137

### How to test React Native apps?

React Native testing uses the same tools as React web testing with some mobile-specific additions:

**Jest** — default test runner, bundled with React Native and Expo.

**React Native Testing Library (RNTL)** — the React Native equivalent of React Testing Library. Provides `render`, `fireEvent`, `waitFor`, and accessibility-based queries:

```js
import { render, fireEvent, screen } from '@testing-library/react-native';
import Counter from './Counter';

test('increments count on press', () => {
  render(<Counter />);
  fireEvent.press(screen.getByText('Increment'));
  expect(screen.getByText('Count: 1')).toBeTruthy();
});
```

**Detox** — end-to-end testing framework by Wix. Runs real app builds on simulators/emulators or real devices. Tests user flows like login, navigation, and form submission.

**Jest + mocks for native modules** — native APIs (Camera, AsyncStorage, etc.) must be mocked in unit tests since they can't run in Node.js:

```js
jest.mock('@react-native-async-storage/async-storage', () =>
  require('@react-native-async-storage/async-storage/jest/async-storage-mock')
);
```

**Related:** [REACT-087 — Testing strategy](./06-testing.md#react-087) | [REACT-089 — React Testing Library](./06-testing.md#react-089)

**Source:** [SudheerJ SDJ-137](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-138

### How to do logging in React Native?

**`console.log`** — the simplest option. Output appears in the Metro bundler terminal and in the browser DevTools when using remote debugging.

```js
console.log('User state:', user);
console.warn('Deprecated usage detected');
console.error('Critical failure:', error);
```

**Flipper** — Facebook's mobile debugging platform. Provides structured log inspection, network inspector, React DevTools integration, and plugin ecosystem for React Native apps.

**React Native Debugger** — standalone Electron app that combines Chrome DevTools + Redux DevTools + React DevTools in one window. Connect via remote JS debugging.

**Reactotron** — third-party desktop app for monitoring React Native apps. Supports logging, Redux state inspection, benchmark tracking, and custom commands.

**Production logging** — for shipped apps, use a service like Sentry, Bugsnag, or Datadog to capture errors and logs from users' devices:

```js
import * as Sentry from '@sentry/react-native';
Sentry.captureException(error);
Sentry.captureMessage('Payment initiated', 'info');
```

**Related:** [REACT-136 — React Native vs React](#react-136)

**Source:** [SudheerJ SDJ-138](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

## REACT-139

### How to debug React Native apps?

**In-app developer menu** — shake the device (or press `Cmd+D` on iOS simulator / `Ctrl+M` on Android emulator) to access: Reload, Debug JS Remotely, Enable Fast Refresh, Performance Monitor, Element Inspector.

**Chrome DevTools (Remote JS Debugging)** — "Debug JS Remotely" opens `http://localhost:8081/debugger-ui`. The JS bundle runs in Chrome's V8 engine, enabling standard breakpoints, call stack inspection, and the console. Note: the JS runs in Chrome, not on-device, so some behavior may differ.

**Flipper** — the recommended debugging environment for newer React Native versions. Connect your device/simulator; Flipper provides:
- React DevTools (component tree, props, state)
- Network inspector (HTTP requests/responses)
- Hermes debugger (breakpoints running on the actual JS engine)
- Crash reporter and device logs

**Hermes Debugger** — when using the Hermes engine (default on Android, available on iOS), use the `chrome://inspect` protocol via Flipper or directly via Metro for true on-device debugging with breakpoints.

**Fast Refresh** — built-in, replaces the old Hot Reloading. Preserves component state across file saves. Enabled by default.

**React Native Testing with Detox** — for catching bugs in real user flows before release, run Detox E2E tests on a simulator.

**Related:** [REACT-136 — React Native vs React](#react-136) | [REACT-138 — Logging](#react-138)

**Source:** [SudheerJ SDJ-139](../../sources/react/github/sudheerj-reactjs-interview-questions/question-map.md)

---

*← [08-redux.md](./08-redux.md) | [10-libraries.md →](./10-libraries.md)*
*[Master Index](../../docs/reference/00-master-index.md)*
