# Web-Utilities
## Shadcn
https://ui.shadcn.com/
## Form-builder 
https://shadcn-form-build.vercel.app/templates/contact/contact

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


## usePersistedState Hook
A custom React hook for persisting state to localStorage. It initializes state from localStorage if data exists and updates localStorage whenever the state changes. This is useful for storing user preferences and data that should persist across page reloads.
```javascript
import { useEffect, useState } from 'react';

export default function usePersistedState<T>(key: string, initialValue: T) {
  const [value, setValue] = useState(() => {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) as T : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue] as const;
}
```
