
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

export function useOutsideClick({ enable, ref, cb }: UseOutsideClickProps) {
  useEffect(() => {
    if(!enable) return
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
  }, [ref, cb]);
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
