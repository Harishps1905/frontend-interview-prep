# top 50 reactjs qus with ans for SSDE

Senior React developers (SSDE level) face questions on React 19 features, performance optimization, concurrent rendering, custom hooks, SSR patterns, and architectural decisions. This top 50 list (2025) consolidates from GreatFrontend, InterviewBit, Reddit senior threads, and React 19 docs, focusing on  senior frontend interviews.[^1][^2][^3][^4][^5]

## React Fundamentals (1-10)

**1. What is React Fiber?**
Fiber is React's reconciliation engine (replaces Stack reconciler). Work-stealing scheduler enables concurrent features, time-slicing, and priority-based rendering. Breaks rendering into units for interruptibility.[^3][^5]

**2. Virtual DOM vs Real DOM?**
Virtual DOM: Lightweight JS objects representing DOM. Diffing produces minimal patches applied to real DOM. Reduces expensive DOM operations from O(n³) to O(n).[^2][^1]

**3. JSX transpilation?**

```
`const element = <h1>Hello</h1>` → `React.createElement('h1', null, 'Hello')`. Babel transforms JSX to function calls.[^1]
```

**4. Keys in lists? Why important?**
`key` helps React identify items across renders for efficient reconciliation. Use stable unique IDs, not array indices (causes unnecessary re-renders/mutations).[^2]

**5. Fragments purpose?**
`<React.Fragment>` or `<>` prevents extra wrapper divs. `<div><li>One</li><li>Two</li></div>` → `<><li>One</li><li>Two</li></>` cleaner DOM.[^1]

**6. Controlled vs Uncontrolled components?**
Controlled: State in React (`value={state}` + `onChange`). Uncontrolled: DOM handles state (`ref.current.value`). Prefer controlled for React patterns.[^1]

**7. Prop drilling solution?**
Context API for medium apps, state management (Zustand/Redux) for complex. Composition (render props/HOC) for targeted sharing.[^4]

**8. PureComponent vs React.memo?**
`PureComponent`: Shallow props/state comparison (class). `React.memo`: Functional equivalent + custom comparison. Use for expensive renders.[^2]

**9. ShouldComponentUpdate vs React.memo?**
`shouldComponentUpdate`: Manual optimization (error-prone). `React.memo`: Automatic shallow comparison. `useMemo`/`useCallback` for hooks.[^4]

**10. React 19 new features?**
Actions (form submissions), React Compiler (auto-memoization), improved hydration, `use` hook (Promises), document metadata API.[^6]

## Hooks Deep Dive (11-25)

**11. useEffect cleanup?**
Return function executes before re-run or unmount: `useEffect(() => { const id = setInterval(...); return () => clearInterval(id); }, [])`.[^1]

**12. useCallback vs useMemo?**
`useCallback`: Memoize functions. `useMemo`: Memoize values. `useCallback(fn) === useMemo(() => fn, deps)`.[^2]

**13. useReducer vs useState?**
`useReducer`: Complex state logic, multiple sub-values, predictable mutations (Redux-like). `useState`: Simple independent values.[^1]

**14. Custom hook rules?**
Call at top level only (no loops/conditions). Name with "use" prefix. Can call other hooks.[^5]

**15. useLayoutEffect vs useEffect?**
`useLayoutEffect`: Sync, blocks paint (DOM measurements). `useEffect`: Async, after paint (side effects). Server: `useLayoutEffect` warns.[^7]

**16. Stale closure fix?**
`useCallback`/`useMemo` dependencies or `useRef` for mutable values: `const countRef = useRef(count); countRef.current = count`.[^4]

**17. useTransition purpose?**
Mark non-urgent updates: `const [startTransition] = useTransition(); startTransition(() => setQuery(value))`. Concurrent, doesn't block UI.[^3]

**18. useDeferredValue?**
Debounces expensive re-renders: `const deferredQuery = useDeferredValue(query)`. UI stays responsive during typing.[^3]

**19. useId for accessibility?**

```
Generates unique IDs: `const id = useId(); <label htmlFor={id}>Name</label><input id={id}/>` SSR-safe.[^3]
```

**20. useSyncExternalStore?**
Subscribe to external stores (Redux, browser APIs): `useSyncExternalStore(store.subscribe, store.getSnapshot)`.[^5]

**21. ESLint React Hooks exhaustive-deps?**
Enforces all dependencies. Fix: Add missing deps or disable specific lines (rarely `// eslint-disable-line`).[^2]

**22. Custom useFetch hook?**

```js
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    fetch(url).then(res => res.json()).then(setData).finally(() => setLoading(false));
  }, [url]);
  return { data, loading };
}
```

**23. useImperativeHandle with forwardRef?**
Expose methods to parent: `useImperativeHandle(ref, () => ({ focus: () => inputRef.current.focus() }))`.[^2]

**24. Hook order dependency?**
Hooks called in same order every render. Violations cause "invalid hook call" errors (conditionals/loops).[^1]

**25. React 19 `use` hook?**
`const promise = use(promise)` reads resolved Promise values during render, suspends if pending.[^6]

## Performance \& Architecture (26-40)

**26. Reconciliation process?**
Fiber walks element tree, diffs with previous, batches mutations. Keys enable efficient list diffing.[^4]

**27. Code splitting strategies?**
`React.lazy` + `Suspense`: `const LazyComp = lazy(() => import('./Comp')); <Suspense fallback={<div>Loading</div>}>`.[^1]

**28. Error boundaries?**
Class component catches JS errors: `static getDerivedStateFromError(error)` + `componentDidCatch(error, info)`. Catch uncaught errors.[^1]

**29. Concurrent rendering benefits?**
Prioritizes urgent updates (user input), time-slices long renders, enables `useTransition`/`Suspense` boundaries.[^3]

**30. Hydration mismatches?**
Server/client render differs → React warns/tears down. Fix: Conditional rendering (`typeof window`), suppressHydrationWarning.[^7]

**31. Bundle optimization tools?**
Webpack Bundle Analyzer, Lighthouse, React DevTools Profiler. Split vendor code, tree-shake unused exports.[^4]

**32. HOC vs Render Props vs Hooks?**
HOC: Reuse logic (with props pollution). Render Props: Flexible (inversion). Hooks: Modern, composable.[^8]

**33. Context vs Redux/Zustand?**
Context: Simple global state. Redux: Predictable, middleware, devtools. Zustand: Minimal, performant, no boilerplate.[^4]

**34. Server Components (React 19)?**
Zero-bundle components rendered only on server. No hooks/interactivity. Access Server Actions/databases directly.[^6]

**35. React.memo gotchas?**
Shallow comparison. Complex objects → `useMemo`. Functions → `useCallback`. Overuse hurts performance.[^2]

**36. Why did you update?**
DevTools plugin detects unnecessary re-renders. Fix: Memoization, split state, callback deps.[^4]

**37. Time slicing?**
Fiber yields to browser every 5ms (`yieldInterval`). Prevents jank during long reconciliations.[^3]

**38. Portal use cases?**
Modals, tooltips outside parent DOM hierarchy: `ReactDOM.createPortal(<Modal/>, document.body)`.[^1]

**39. State updater functions?**
`setCount(prev => prev + 1)` ensures latest state, avoids stale closures.[^1]

**40. React.StrictMode?**
Development: Double-invokes effects/constructors to catch side effects, deprecated APIs.[^1]

## Advanced Patterns (41-50)

**41. Compound Components pattern?**
Share state implicitly: `<Select><Option>A</Option></Select>` via `useContext`/`cloneElement`.[^4]

**42. Custom useLocalStorage hook?**

```js
function useLocalStorage(key, initial) {
  const [value, setValue] = useState(() => localStorage.getItem(key) || initial);
  useEffect(() => localStorage.setItem(key, JSON.stringify(value)), [key, value]);
  return [value, setValue];
}
```

**43. Prefetching with React Router?**
`createBrowserRouter` + `loader` functions prefetch data. `defer()` for streaming Suspense.[^3]

**44. Micro-frontends with Module Federation?**
Webpack 5 shares remotes: Load React apps dynamically, single SPA instance.[^4]

**45. Concurrent SSR with React 19?**
`streamServerRendering` + `Suspense` boundaries. Partial HTML streaming, client hydration.[^6]

**46. Custom useDebounce hook?**

```js
function useDebounce(value, delay) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);
  return debounced;
}
```

**47. Lifting state up vs Context?**
Lift for close siblings. Context for deeper trees (with `useMemo` to avoid re-renders).[^1]

**48. React Query vs SWR?**
React Query: Full caching, mutations, optimistic updates, devtools. SWR: Simpler, stale-while-revalidate.[^4]

**49. Testing custom hooks?**
`@testing-library/react-hooks`: `renderHook(() => useCustomHook())`, `rerender` for prop changes.[^5]

**50. Migration class → hooks?**
`componentDidMount/Update` → `useEffect`. `getDerivedStateFromProps` → `useEffect` + updater funcs. HOCs → custom hooks.[^1]

##  SSDE Prep Priority

Master concurrent features (`useTransition`), custom hooks, React 19 Server Components, performance profiling. Practice live-coding complex forms with validation/optimistic updates.[^6][^3][^4]
