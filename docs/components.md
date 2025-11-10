# Components Guide

This guide explains how to create and organize components in this Next.js boilerplate.

## Component Organization

Components are stored in `src/components/` with the following structure:

```
src/components/
├── ui/           # Basic UI components (buttons, inputs, cards)
├── forms/        # Form-related components
├── shared/       # Shared components across multiple pages
└── layout/       # Layout components (header, footer, sidebar)
```

## Component Types

### Server Components (Default)

Server Components render on the server and don't include client-side JavaScript.

**When to use:**
- Displaying static content
- Fetching data from databases or APIs
- No interactivity needed
- Better performance and SEO

**Example:**
```typescript
// src/components/ui/Card.tsx
interface CardProps {
  title: string
  description: string
}

export default function Card({ title, description }: CardProps) {
  return (
    <div className="p-6 border rounded-lg shadow-sm">
      <h3 className="text-xl font-bold mb-2">{title}</h3>
      <p className="text-gray-600">{description}</p>
    </div>
  )
}
```

### Client Components

Client Components run in the browser and can use React hooks and browser APIs.

**When to use:**
- Interactive components (buttons, forms)
- Using React hooks (useState, useEffect, etc.)
- Event handlers (onClick, onChange, etc.)
- Browser APIs (window, localStorage, etc.)

**Example:**
```typescript
// src/components/ui/Counter.tsx
'use client'

import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div className="flex items-center gap-4">
      <button
        onClick={() => setCount(count - 1)}
        className="px-4 py-2 bg-red-500 text-white rounded"
      >
        -
      </button>
      <span className="text-2xl font-bold">{count}</span>
      <button
        onClick={() => setCount(count + 1)}
        className="px-4 py-2 bg-green-500 text-white rounded"
      >
        +
      </button>
    </div>
  )
}
```

## Common Components

### Button Component

```typescript
// src/components/ui/Button.tsx
'use client'

import { ButtonHTMLAttributes } from 'react'

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'danger'
  size?: 'sm' | 'md' | 'lg'
  children: React.ReactNode
}

export default function Button({
  variant = 'primary',
  size = 'md',
  children,
  className = '',
  ...props
}: ButtonProps) {
  const baseStyles = 'rounded font-medium transition-colors'

  const variants = {
    primary: 'bg-blue-500 hover:bg-blue-600 text-white',
    secondary: 'bg-gray-200 hover:bg-gray-300 text-gray-900',
    danger: 'bg-red-500 hover:bg-red-600 text-white'
  }

  const sizes = {
    sm: 'px-3 py-1.5 text-sm',
    md: 'px-4 py-2 text-base',
    lg: 'px-6 py-3 text-lg'
  }

  return (
    <button
      className={`${baseStyles} ${variants[variant]} ${sizes[size]} ${className}`}
      {...props}
    >
      {children}
    </button>
  )
}
```

**Usage:**
```typescript
import Button from '@/components/ui/Button'

<Button variant="primary" size="md" onClick={() => console.log('clicked')}>
  Click Me
</Button>
```

### Input Component

```typescript
// src/components/ui/Input.tsx
'use client'

import { InputHTMLAttributes, forwardRef } from 'react'

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label?: string
  error?: string
}

const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ label, error, className = '', ...props }, ref) => {
    return (
      <div className="w-full">
        {label && (
          <label className="block text-sm font-medium mb-1">
            {label}
          </label>
        )}
        <input
          ref={ref}
          className={`w-full px-3 py-2 border rounded focus:outline-none focus:ring-2 focus:ring-blue-500 ${
            error ? 'border-red-500' : 'border-gray-300'
          } ${className}`}
          {...props}
        />
        {error && (
          <p className="mt-1 text-sm text-red-500">{error}</p>
        )}
      </div>
    )
  }
)

Input.displayName = 'Input'

export default Input
```

### Loading Spinner

```typescript
// src/components/ui/Spinner.tsx
export default function Spinner({ size = 'md' }: { size?: 'sm' | 'md' | 'lg' }) {
  const sizes = {
    sm: 'w-4 h-4',
    md: 'w-8 h-8',
    lg: 'w-12 h-12'
  }

  return (
    <div className={`${sizes[size]} animate-spin rounded-full border-2 border-gray-300 border-t-blue-500`} />
  )
}
```

## Component Patterns

### Composition Pattern

```typescript
// src/components/ui/Card.tsx
interface CardProps {
  children: React.ReactNode
  className?: string
}

function Card({ children, className = '' }: CardProps) {
  return (
    <div className={`border rounded-lg shadow-sm ${className}`}>
      {children}
    </div>
  )
}

function CardHeader({ children }: { children: React.ReactNode }) {
  return <div className="p-6 border-b">{children}</div>
}

function CardBody({ children }: { children: React.ReactNode }) {
  return <div className="p-6">{children}</div>
}

function CardFooter({ children }: { children: React.ReactNode }) {
  return <div className="p-6 border-t bg-gray-50">{children}</div>
}

Card.Header = CardHeader
Card.Body = CardBody
Card.Footer = CardFooter

export default Card
```

**Usage:**
```typescript
<Card>
  <Card.Header>
    <h2>Card Title</h2>
  </Card.Header>
  <Card.Body>
    <p>Card content goes here</p>
  </Card.Body>
  <Card.Footer>
    <Button>Action</Button>
  </Card.Footer>
</Card>
```

### Render Props Pattern

```typescript
// src/components/shared/DataFetcher.tsx
'use client'

import { useState, useEffect } from 'react'

interface DataFetcherProps<T> {
  url: string
  children: (data: T | null, loading: boolean, error: Error | null) => React.ReactNode
}

export default function DataFetcher<T>({ url, children }: DataFetcherProps<T>) {
  const [data, setData] = useState<T | null>(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<Error | null>(null)

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false))
  }, [url])

  return <>{children(data, loading, error)}</>
}
```

**Usage:**
```typescript
<DataFetcher<User> url="/api/user">
  {(data, loading, error) => {
    if (loading) return <Spinner />
    if (error) return <div>Error: {error.message}</div>
    if (!data) return null
    return <div>{data.name}</div>
  }}
</DataFetcher>
```

## Styling Components

### Using Tailwind Classes

```typescript
export default function Alert({ message }: { message: string }) {
  return (
    <div className="p-4 bg-blue-50 border border-blue-200 rounded-lg">
      <p className="text-blue-800">{message}</p>
    </div>
  )
}
```

### Conditional Classes

```typescript
export default function Badge({
  status
}: {
  status: 'success' | 'warning' | 'error'
}) {
  const statusStyles = {
    success: 'bg-green-100 text-green-800',
    warning: 'bg-yellow-100 text-yellow-800',
    error: 'bg-red-100 text-red-800'
  }

  return (
    <span className={`px-2 py-1 rounded text-sm ${statusStyles[status]}`}>
      {status}
    </span>
  )
}
```

## Best Practices

1. **TypeScript Types**: Always define prop types with TypeScript interfaces
2. **Default Props**: Use default parameters for optional props
3. **Accessibility**: Include ARIA attributes and semantic HTML
4. **Performance**: Use `React.memo()` for expensive components
5. **Naming**: Use PascalCase for component names
6. **File Organization**: One component per file
7. **Props Spreading**: Use carefully with `{...props}` for flexibility
8. **Server First**: Default to Server Components unless interactivity is needed

## Testing Components

```typescript
// src/components/ui/Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react'
import Button from './Button'

describe('Button', () => {
  it('renders children', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByText('Click me')).toBeInTheDocument()
  })

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn()
    render(<Button onClick={handleClick}>Click me</Button>)
    fireEvent.click(screen.getByText('Click me'))
    expect(handleClick).toHaveBeenCalledTimes(1)
  })
})
```

## Component Libraries

Consider adding these libraries for pre-built components:

- **[shadcn/ui](https://ui.shadcn.com/)** - Copy-paste component library
- **[Radix UI](https://www.radix-ui.com/)** - Unstyled accessible components
- **[Headless UI](https://headlessui.com/)** - Unstyled components by Tailwind
- **[React Aria](https://react-spectrum.adobe.com/react-aria/)** - Accessible components

## Resources

- [React Component Patterns](https://www.patterns.dev/react)
- [Next.js Server Components](https://nextjs.org/docs/app/building-your-application/rendering/server-components)
- [Tailwind CSS Components](https://tailwindcomponents.com/)
