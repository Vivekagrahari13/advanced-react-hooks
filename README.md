# Advanced React Hooks

A practice project from the bootcamp for learning the advanced React hooks one by one. This README is updated as each hook is covered.

## Hooks

| Hook | Status |
| --- | --- |
| `useCallback` | Done |
| `useMemo` | Not started |
| `useRef` | Not started |

## useCallback

`useCallback` caches a function between renders so React returns the same function until its dependencies change.

### The setup

`Parent` holds two pieces of state, `age` and `salary`, each with its own increment button:

```
Parent
 ├─ Title
 ├─ Count (Age)
 ├─ Button (Increment Age)
 ├─ Count (Salary)
 └─ Button (Increment Salary)
```

Every component logs `Rendering ...` to the console, so you can see which components re-render.

### The problem observed

Clicking **Increment Age** re-renders every component, including `Title`, the Salary `Count` and the Salary `Button`, even though none of their data changed. This is because:

- When state changes, `Parent` re-renders, and React re-renders all of its children by default.
- The handlers (`incrementAge`, `incrementSalary`) are created again on every render, so each render gives children a new function.

### The fix

- Wrapped `Title`, `Count` and `Button` in `React.memo`, so each skips re-rendering when its props are the same as last time.
- Wrapped `incrementAge` and `incrementSalary` in `useCallback`, so `Button` receives the same function between renders. Without this, `memo` is defeated because a new function counts as a changed prop.

```jsx
const incrementAge = useCallback(() => {
  setAge(age + 1);
}, [age]);
```

The dependency array lists the values the function reads. If `age` were left out, the function would keep using an old value (stale closure).

### Result

| Click | Re-renders | Skipped |
| --- | --- | --- |
| Increment Age | `Parent`, Age `Count`, Age `Button` | `Title`, Salary `Count`, Salary `Button` |
| Increment Salary | `Parent`, Salary `Count`, Salary `Button` | `Title`, Age `Count`, Age `Button` |

The button for the value that changed still re-renders, because its handler depends on that value and is recreated. Using the updater form, `setAge((a) => a + 1)` with an empty dependency array, keeps the function stable so the button never re-renders.

### Key points

- `React.memo` caches a component, `useCallback` caches a function, and `useMemo` caches a value.
- `useCallback` alone does nothing unless the child is wrapped in `React.memo`.
- Memoization has a cost, so use it where a re-render problem is real.

## Notes

- Logs may appear twice in development when `<StrictMode>` is on. It is currently removed from `main.jsx` to keep the console readable.

## Getting started

```bash
npm install
npm run dev
```

The app runs at `http://localhost:5173`.

## Project structure

```
src/
  components/
    Parent.jsx   # state and handlers
    Title.jsx
    Count.jsx
    Button.jsx
  App.jsx
```
