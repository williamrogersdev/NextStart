# Architecture

This document describes the architecture and design decisions of the Next.js AI Boilerplate.

## Overview

This project follows a modern Next.js architecture with the App Router, emphasizing:

- **Server-first approach** - Default to Server Components
- **Type safety** - TypeScript throughout
- **Modular structure** - Organized by feature and responsibility
- **AI-optimized** - Clear structure for AI coding agents

## Technology Stack

### Core Framework
- **Next.js 16** - React framework with App Router
- **React 19** - UI library with latest features
- **TypeScript** - Type-safe JavaScript

### Styling
- **Tailwind CSS v4** - Utility-first CSS framework
- **CSS Modules** - For component-specific styles when needed

### Development Tools
- **ESLint** - Code linting and quality
- **TypeScript Compiler** - Type checking

## Directory Structure

```
src/
├── app/              # Next.js App Router
│   ├── (routes)/    # Route groups (optional)
│   ├── api/         # API routes
│   ├── layout.tsx   # Root layout
│   ├── page.tsx     # Home page
│   └── globals.css  # Global styles
│
├── components/       # Reusable React components
│   ├── ui/          # Basic UI components
│   ├── forms/       # Form components
│   └── shared/      # Shared components
│
├── lib/             # Utility functions and helpers
│   ├── utils.ts     # General utilities
│   ├── api.ts       # API client
│   └── constants.ts # App constants
│
└── types/           # TypeScript type definitions
    ├── index.ts     # Shared types
    └── api.ts       # API types
```

## Key Concepts

### Server Components vs Client Components

**Server Components (Default)**
- Render on the server
- Can access backend resources directly
- No JavaScript sent to client
- Better performance and SEO

```typescript
// src/app/page.tsx - Server Component
export default async function HomePage() {
  const data = await fetch('https://api.example.com/data')
  return <div>{/* ... */}</div>
}
```

**Client Components**
- Render on the client
- Can use React hooks and browser APIs
- Add `'use client'` directive

```typescript
// src/components/ui/Button.tsx - Client Component
'use client'

import { useState } from 'react'

export default function Button() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(count + 1)}>{count}</button>
}
```

### Data Fetching Patterns

**Server-Side Fetching**
```typescript
// Directly in Server Components
async function getData() {
  const res = await fetch('https://api.example.com/data', {
    cache: 'no-store' // for dynamic data
  })
  return res.json()
}

export default async function Page() {
  const data = await getData()
  return <div>{/* Use data */}</div>
}
```

**Client-Side Fetching**
```typescript
'use client'

import { useEffect, useState } from 'react'

export default function ClientComponent() {
  const [data, setData] = useState(null)

  useEffect(() => {
    fetch('/api/data')
      .then(res => res.json())
      .then(setData)
  }, [])

  return <div>{/* Use data */}</div>
}
```

### Routing

**File-Based Routing**
- `src/app/page.tsx` → `/`
- `src/app/about/page.tsx` → `/about`
- `src/app/blog/[slug]/page.tsx` → `/blog/:slug`

**Dynamic Routes**
```typescript
// src/app/blog/[slug]/page.tsx
export default function BlogPost({ params }: { params: { slug: string } }) {
  return <h1>Post: {params.slug}</h1>
}
```

**Route Groups**
```
src/app/
├── (marketing)/
│   ├── about/
│   └── contact/
└── (dashboard)/
    ├── profile/
    └── settings/
```

### API Routes

**Creating API Routes**
```typescript
// src/app/api/users/route.ts
export async function GET() {
  const users = await fetchUsers()
  return Response.json(users)
}

export async function POST(request: Request) {
  const body = await request.json()
  const user = await createUser(body)
  return Response.json(user, { status: 201 })
}
```

**Dynamic API Routes**
```typescript
// src/app/api/users/[id]/route.ts
export async function GET(
  request: Request,
  { params }: { params: { id: string } }
) {
  const user = await fetchUser(params.id)
  return Response.json(user)
}
```

## State Management

### Server State
- Use Server Components when possible
- Fetch data directly in components
- No need for client-side state management

### Client State
- **Local state**: `useState` for component-level state
- **Global state**: Context API for shared state
- **Form state**: React Hook Form (if added)
- **Server state**: React Query or SWR (if added)

## Performance Optimization

### Image Optimization
```typescript
import Image from 'next/image'

<Image
  src="/image.jpg"
  alt="Description"
  width={500}
  height={300}
  priority // for above-the-fold images
/>
```

### Font Optimization
```typescript
import { Inter } from 'next/font/google'

const inter = Inter({ subsets: ['latin'] })

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

### Code Splitting
- Automatic with Next.js App Router
- Use dynamic imports for large components:

```typescript
import dynamic from 'next/dynamic'

const HeavyComponent = dynamic(() => import('@/components/HeavyComponent'), {
  loading: () => <p>Loading...</p>
})
```

## Security Best Practices

1. **Environment Variables**
   - Use `.env.local` for secrets
   - Never commit `.env.local` to Git
   - Use `NEXT_PUBLIC_` prefix only for public variables

2. **API Routes**
   - Validate all inputs
   - Use proper authentication/authorization
   - Implement rate limiting

3. **Data Validation**
   - Validate on both client and server
   - Use TypeScript for type safety
   - Consider Zod or similar for runtime validation

## Deployment Considerations

### Build Output
```bash
npm run build
```

Generates:
- `.next/` - Build output
- Static pages pre-rendered
- Server-side code bundled

### Environment-Specific Config
- Development: `.env.local`
- Production: Set via platform (Vercel, etc.)

### Caching Strategy
- Static pages: Cached indefinitely
- Dynamic pages: ISR or no-cache
- API routes: Custom cache headers

## Design Decisions

### Why App Router?
- Better performance with Server Components
- Simplified data fetching
- Built-in loading and error states
- Future of Next.js

### Why `src/` Directory?
- Cleaner root directory
- Clear separation of source code
- Better organization for larger projects
- Standard convention in React ecosystem

### Why Tailwind CSS?
- Rapid development
- Consistent design system
- Small bundle size with purging
- Easy to customize

### Why TypeScript?
- Type safety catches bugs early
- Better IDE support and autocomplete
- Self-documenting code
- Required for modern AI-assisted development

## Future Considerations

When scaling this boilerplate, consider adding:

- **Testing**: Jest + React Testing Library
- **State Management**: Zustand or Jotai for complex state
- **API Client**: React Query or SWR for data fetching
- **Form Handling**: React Hook Form + Zod
- **UI Library**: shadcn/ui or Radix UI
- **Database**: Prisma + PostgreSQL
- **Authentication**: NextAuth.js or Clerk
- **Analytics**: Vercel Analytics or PostHog

## References

- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
