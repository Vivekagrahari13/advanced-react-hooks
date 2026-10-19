# Advanced React Hooks

A practice project from the bootcamp for learning the advanced React hooks one by one. This README is updated as each hook is covered.

## Hooks

| Hook | Status |
| --- | --- |
| `useCallback` | In progress |
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

### Next steps

- Wrap `Title`, `Count` and `Button` in `React.memo` so unchanged props skip re-rendering
- Wrap the handlers in `useCallback` so `memo` isn't defeated by new function references
- Compare the console logs before and after

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
