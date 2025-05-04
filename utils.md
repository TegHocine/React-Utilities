# React-Utilities
## isNumber Function
The `isNumber` function checks if the provided value is a valid number. It accepts both numeric and string representations of numbers. The function returns `true` if the value is a number or a string that can be converted to a finite number; otherwise, it returns `false`.

```typescript
const isNumber = (v: string | number) =>
(typeof v === "number" && v - v === 0) ||
(typeof v === "string" && Number.isFinite(+v) && v.trim() !== "");
```


## Axios API Setup
This setup uses Axios to handle HTTP requests with automatic token management and global error handling. The base URL is dynamically set using environment variables.

```typescript

import axios from "axios";
import { getInitialValue } from "../utils";

const baseURL = "";

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
    const storedToken = window.localStorage.getItem("token");
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

## localStorage
Helper functions for localStorage with improved names and typings

```typescript

export const saveToLocalStorage = (key: string, value: unknown): void => {
  try {
    window.localStorage.setItem(key, JSON.stringify(value));
  } catch (error) {
    console.error("Failed to save to localStorage:", error);
  }
};

export const getFromLocalStorage = <T>(key: string): T | undefined => {
  try {
    const item = window.localStorage.getItem(key);
    return item ? (JSON.parse(item) as T) : undefined;
  } catch (error) {
    console.error("Failed to retrieve from localStorage:", error);
    return undefined;
  }
};

export const removeFromLocalStorage = (key: string): void => {
  try {
    window.localStorage.removeItem(key);
  } catch (error) {
    console.error("Failed to remove from localStorage:", error);
  }
};

```
