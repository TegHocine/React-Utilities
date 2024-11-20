
# React-Hooks
## `useOutsideClick` Hook
A custom React hook to detect clicks outside of a specified element. This can be particularly useful for closing dropdowns, modals, or any popover when the user clicks outside of the targeted area.

```typescript
import { useEffect } from 'react';

interface UseOutsideClickProps {
  enabled: boolean;
  ref: React.MutableRefObject<HTMLDivElement | null>;
  cb: () => void;
}

export function useOutsideClick({ enabled, ref, cb }: UseOutsideClickProps) {
  useEffect(() => {
    if (!enabled) return;

    const element = ref.current;
    if (!element) return;

    function handle(e: MouseEvent) {
      if (!element.contains(e.target as Node)) {
        cb();
      }
    }

    document.addEventListener('click', handle);
    return () => {
      document.removeEventListener('click', handle);
    };
  }, [enabled, ref, cb]);
}
```
## usePersistedState Hook
A custom React hook for persisting state to localStorage. It initializes state from localStorage if data exists and updates localStorage whenever the state changes. This is useful for storing user preferences and data that should persist across page reloads.

```typescript
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
## useDebounce Hook
The useDebounce hook is a custom React hook that delays the update of a value until after a specified delay time has passed.

```typescript
import { useEffect, useState } from 'react';

export const useDebounce = <T>(value: T, delay = 500) => {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);

  useEffect(() => {
    const timeout = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(timeout);
  }, [value, delay]);

  return debouncedValue;
}
```

