# AGENTS.md

This file provides instructions and context for AI coding agents working on this Next.js project.

## Project Overview

This is a Next.js 16 boilerplate optimized for AI-assisted development. It uses:
- **Framework**: Next.js 16 with App Router
- **Language**: TypeScript (strict mode)
- **Styling**: Tailwind CSS v4
- **Package Manager**: pnpm (recommended)
- **React**: Version 19.2.0

## Setup Commands

```bash
# Install dependencies
pnpm install

# Start development server (runs on http://localhost:3000)
pnpm run dev

# Build for production
pnpm run build

# Start production server
pnpm run start

# Run linting
pnpm run lint
```

## Project Structure

```
nextjs-boilerplate/
├── src/
│   ├── app/            # Next.js App Router
│   │   ├── layout.tsx  # Root layout component
│   │   ├── page.tsx    # Home page
│   │   ├── globals.css # Global styles with Tailwind
│   │   └── favicon.ico # Site favicon
│   ├── components/     # Reusable React components
│   │   ├── ui/         # UI components (buttons, inputs, etc.)
│   │   └── shared/     # Shared components across pages
│   ├── lib/            # Utility functions and helpers
│   │   ├── utils.ts    # General utility functions
│   │   └── api.ts      # API client functions
│   └── types/          # TypeScript type definitions
│       └── index.ts    # Shared types
├── docs/               # Project documentation
│   ├── architecture.md # Architecture decisions
│   └── api.md          # API documentation
├── public/             # Static assets (images, fonts, etc.)
├── package.json        # Dependencies and scripts
├── tsconfig.json       # TypeScript configuration
├── next.config.ts      # Next.js configuration
└── eslint.config.mjs   # ESLint configuration
```

## Code Style Guidelines

### TypeScript
- **Always use TypeScript** for all new files (.ts, .tsx)
- **Enable strict mode** - respect all TypeScript strict checks
- **Explicit types** - prefer explicit return types on functions
- **Avoid `any`** - use `unknown` if type is truly unknown
- **Interface over type** - use `interface` for object shapes, `type` for unions/intersections

### React/Next.js Conventions
- **Server Components by default** - only add `'use client'` when necessary
- **Functional components** - no class components
- **Async Server Components** - leverage async/await in Server Components
- **Colocation** - keep related components, styles, and tests together
- **File naming**:
  - Components: PascalCase (e.g., `UserProfile.tsx`)
  - Pages: lowercase (e.g., `page.tsx`, `layout.tsx`)
  - Utilities: camelCase (e.g., `formatDate.ts`)

### Styling
- **Tailwind-first** - use Tailwind utility classes for styling
- **No inline styles** - avoid style props unless absolutely necessary
- **CSS Modules** - if custom CSS needed, use CSS Modules
- **Responsive design** - mobile-first approach with Tailwind breakpoints

### Code Quality
- **ESLint rules** - follow the ESLint configuration
- **No console.log** - remove debug logs before committing
- **Error handling** - always handle errors gracefully
- **Accessibility** - use semantic HTML and ARIA attributes

## Development Workflow

### Creating New Pages
1. Create a new folder in `src/app/` directory (e.g., `src/app/about/`)
2. Add a `page.tsx` file in that folder
3. Export a default React component
4. Optionally add `layout.tsx` for page-specific layouts
5. Add `loading.tsx` for loading states
6. Add `error.tsx` for error boundaries

Example:
```typescript
// src/app/about/page.tsx
export default function AboutPage() {
  return (
    <div>
      <h1>About Us</h1>
    </div>
  )
}
```

### Creating New Components
1. Create components in `src/components/` directory
2. Organize by feature or type (e.g., `src/components/ui/`, `src/components/forms/`)
3. Use Server Components by default
4. Add `'use client'` only when using:
   - React hooks (useState, useEffect, etc.)
   - Event handlers (onClick, onChange, etc.)
   - Browser APIs (window, document, etc.)
5. Import using path alias: `import Button from '@/components/ui/Button'`

Example:
```typescript
// src/components/ui/Button.tsx
'use client'

interface ButtonProps {
  onClick: () => void
  children: React.ReactNode
}

export default function Button({ onClick, children }: ButtonProps) {
  return (
    <button onClick={onClick} className="px-4 py-2 bg-blue-500 text-white">
      {children}
    </button>
  )
}
```

### Creating Utility Functions
1. Add utility functions to `src/lib/` directory
2. Keep functions pure and testable
3. Export functions for reuse
4. Import using path alias: `import { formatDate } from '@/lib/utils'`

Example:
```typescript
// src/lib/utils.ts
export function formatDate(date: Date): string {
  return date.toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}
```

### Creating Type Definitions
1. Add shared types to `src/types/` directory
2. Use interfaces for object shapes
3. Export types for reuse across the app

Example:
```typescript
// src/types/index.ts
export interface User {
  id: string
  name: string
  email: string
  createdAt: Date
}

export type UserRole = 'admin' | 'user' | 'guest'
```

### API Routes
1. Create API routes in `src/app/api/` directory
2. Each route should be in a `route.ts` file
3. Export named functions: GET, POST, PUT, DELETE, etc.

Example:
```typescript
// src/app/api/hello/route.ts
export async function GET() {
  return Response.json({ message: 'Hello World' })
}
```

## Testing Instructions

Currently, this boilerplate does not include a testing setup. When adding tests:

1. **Install testing library**:
   ```bash
   pnpm add -D jest @testing-library/react @testing-library/jest-dom
   ```

2. **Add test scripts** to `package.json`:
   ```json
   {
     "scripts": {
       "test": "jest",
       "test:watch": "jest --watch"
     }
   }
   ```

3. **Create tests** alongside components with `.test.tsx` extension

4. **Run tests** before committing:
   ```bash
   pnpm run test
   pnpm run lint
   pnpm run build
   ```

## Environment Variables

1. Create `.env.local` for local development (gitignored by default)
2. Use `NEXT_PUBLIC_` prefix for client-side variables
3. Server-side variables don't need prefix
4. Access variables: `process.env.VARIABLE_NAME`

Example `.env.local`:
```bash
# Client-side (accessible in browser)
NEXT_PUBLIC_API_URL=https://api.example.com

# Server-side only
DATABASE_URL=postgresql://...
SECRET_KEY=abc123
```

## Common Tasks

### Adding a New Package
```bash
pnpm add package-name
# or for dev dependencies
pnpm add -D package-name
```

### Updating Next.js
```bash
pnpm add next@latest react@latest react-dom@latest
```

### Checking for Type Errors
```bash
pnpm exec tsc --noEmit
```

### Analyzing Bundle Size
```bash
pnpm run build
# Next.js will show bundle sizes in output
```

## Deployment

### Vercel (Recommended)
1. Push code to GitHub/GitLab/Bitbucket
2. Import project on [Vercel](https://vercel.com)
3. Vercel auto-detects Next.js and configures build
4. Add environment variables in Vercel dashboard

### Docker
1. Add a `Dockerfile`:
   ```dockerfile
   FROM node:18-alpine
   WORKDIR /app
   RUN npm install -g pnpm
   COPY package.json pnpm-lock.yaml ./
   RUN pnpm install --frozen-lockfile
   COPY . .
   RUN pnpm run build
   CMD ["pnpm", "start"]
   ```
2. Build: `docker build -t nextjs-app .`
3. Run: `docker run -p 3000:3000 nextjs-app`

### Static Export (if applicable)
1. Update `next.config.ts`:
   ```typescript
   const nextConfig = {
     output: 'export',
   }
   ```
2. Run `pnpm run build`
3. Deploy `out/` folder to any static host

## Troubleshooting

### Port 3000 Already in Use
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9
# Or use different port
pnpm run dev -- -p 3001
```

### Type Errors
- Run `pnpm exec tsc --noEmit` to see all TypeScript errors
- Check `tsconfig.json` for proper configuration
- Ensure all dependencies have type definitions

### Tailwind Styles Not Working
- Check `globals.css` has Tailwind directives
- Restart dev server after config changes
- Clear `.next` folder: `rm -rf .next`

## Security Considerations

- **Never commit secrets** - use `.env.local` for sensitive data
- **Validate user input** - always sanitize and validate on server
- **Use environment variables** - for API keys and secrets
- **HTTPS in production** - ensure SSL/TLS is enabled
- **Dependencies** - regularly update packages for security patches

## AI Agent Tips

When working on this project:
1. **Read existing code first** - understand patterns before adding new code
2. **Maintain consistency** - follow established patterns and conventions
3. **Test changes** - run `pnpm run dev` to verify changes work
4. **Check types** - ensure no TypeScript errors before completing tasks
5. **Run linter** - execute `pnpm run lint` before finishing
6. **Consider Server Components** - use client components only when needed
7. **Keep it simple** - avoid over-engineering solutions
8. **Document complex logic** - add comments for non-obvious code

## Resources

- [Next.js Docs](https://nextjs.org/docs)
- [React Docs](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Next.js Examples](https://github.com/vercel/next.js/tree/canary/examples)
