# React-Utilities

## React Query Setup with Mutation Cache

This setup uses QueryClient from React Query with a custom MutationCache. It automatically invalidates queries after a successful mutation, ensuring fresh data.


```javascript
const queryClient = new QueryClient({
  mutationCache: new MutationCache({
    onSuccess: () => {
      queryClient.invalidateQueries();
    },
  }),
});
```

## isNumber Function

The `isNumber` function checks if the provided value is a valid number. It accepts both numeric and string representations of numbers. The function returns `true` if the value is a number or a string that can be converted to a finite number; otherwise, it returns `false`.


```javascript
const isNumber = (v) =>
(typeof v === "number" && v - v === 0) ||
(typeof v === "string" && Number.isFinite(+v) && v.trim() !== "");
```
