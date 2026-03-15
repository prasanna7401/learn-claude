---
description: Scaffold a new React component with TypeScript and Tailwind
argument-hint: <ComponentName>
allowed-tools: [Write, Read, Glob]
---

Create a new React component named **$ARGUMENTS**.

1. Create the file `src/components/$ARGUMENTS.tsx` with:
   - A TypeScript interface for props (named `$ARGUMENTSProps`)
   - A default-exported functional component
   - Basic Tailwind CSS classes for layout
   - A placeholder `children` prop

Example structure to follow:

```tsx
interface $ARGUMENTSProps {
  children?: React.ReactNode;
  className?: string;
}

export default function $ARGUMENTS({ children, className }: $ARGUMENTSProps) {
  return (
    <div className={`... ${className ?? ""}`}>
      {children}
    </div>
  );
}
```

2. After creating the file, confirm the path and summarize what was generated.
