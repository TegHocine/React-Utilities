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

