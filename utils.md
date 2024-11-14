# React-Utilities
## React Query Setup with Mutation Cache
This setup uses QueryClient from React Query with a custom MutationCache. It automatically invalidates queries after a successful mutation, ensuring fresh data.

```typescript
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

```typescript
const isNumber = (v) =>
(typeof v === "number" && v - v === 0) ||
(typeof v === "string" && Number.isFinite(+v) && v.trim() !== "");
```


## Axios API Setup
This setup uses Axios to handle HTTP requests with automatic token management and global error handling. The base URL is dynamically set using environment variables.

```typescript

import axios from "axios";
import { getInitialValue } from "../utils";

const baseURL = import.meta.env.VITE_API_BASE_URL;

const api = axios.create({
  baseURL,
  headers: {
    "Content-Type": "application/json",
    Accept: "application/json",
  },
});

// Request Interceptor
api.interceptors.request.use(
  (config) => {
    const storedToken = getInitialValue(null, "token");
    if (storedToken) {
      config.headers["Authorization"] = `Bearer ${storedToken}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response Interceptor
api.interceptors.response.use(
  (response) => response,
  (error) => {
    // Uncomment the below section to handle 401 errors
    // if (error?.response?.status === 401) {
    //     localStorage.removeItem("token");
    //     localStorage.removeItem("user");
    //     window.location.href = "/login";
    // }
    return Promise.reject(error);
  }
);

export { api };

```
