# TypeScript Code Review Checklist

## Type Safety
- [ ] No `any` types — use `unknown` or proper generics
- [ ] Strict mode enabled in tsconfig.json
- [ ] Discriminated unions for state management
- [ ] Proper null checks (optional chaining, nullish coalescing)
- [ ] Return types on exported functions

## Error Handling
- [ ] No empty catch blocks
- [ ] Error types checked (instanceof)
- [ ] Custom error classes for domain errors
- [ ] Try-catch at boundary layers only

## React / Component Patterns (if applicable)
- [ ] No inline function definitions in JSX (memoize)
- [ ] useCallback / useMemo where needed
- [ ] Proper key props in lists
- [ ] No state derived from props (use useMemo)
- [ ] Cleanup in useEffect return

## Common Pitfalls
- [ ] No `==` comparisons — always `===`
- [ ] Array methods: prefer immutable (.map, .filter) over .push
- [ ] No floating promises — always await or void
- [ ] Proper enum usage (const enum for performance)
