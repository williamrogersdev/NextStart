# Claude Code Guide for Next.js Boilerplate

This document provides Claude Code-specific guidance for working with this Next.js boilerplate project.

## Project Overview

This is a production-ready Next.js 16 boilerplate with:
- **App Router** architecture
- **React 19** with modern features
- **TypeScript** for type safety
- **Tailwind CSS v4** for styling
- **pnpm** as package manager

## Quick Commands

```bash
# Development
pnpm run dev          # Start dev server on localhost:3000
pnpm run build        # Production build
pnpm run start        # Start production server
pnpm run lint         # Run ESLint

# Package Management
pnpm install          # Install dependencies
pnpm add <package>    # Add new package
```

## Code Style & Conventions

### TypeScript
- Use TypeScript for all new files
- Define types in `src/types/` for shared types
- Use interfaces for object shapes, types for unions/primitives
- Enable strict mode (already configured)

### React Components
- Use functional components with hooks
- Prefer Server Components by default (App Router)
- Add `"use client"` directive only when needed
- Use async/await for Server Components data fetching

### File Naming
- Components: PascalCase (e.g., `Button.tsx`)
- Utilities: camelCase (e.g., `formatDate.ts`)
- Pages: lowercase with dashes (e.g., `about-us/page.tsx`)

### Tailwind CSS
- Use Tailwind utility classes
- Add custom styles in `src/app/globals.css` if needed
- Follow mobile-first responsive design

## Project Structure

```
src/
├── app/              # Next.js App Router
│   ├── layout.tsx    # Root layout (metadata, fonts)
│   ├── page.tsx      # Home page
│   └── globals.css   # Global styles
├── components/       # Reusable components
├── lib/              # Utility functions
└── types/            # TypeScript definitions
```

## Common Tasks

### Creating a New Page
```bash
# Create route directory
mkdir -p src/app/about
# Create page component
```
Then create `src/app/about/page.tsx` with a Server Component.

### Adding a Component
Create in `src/components/` following this pattern:
```typescript
interface ComponentProps {
  // props here
}

export function ComponentName({ }: ComponentProps) {
  return (
    <div>
      {/* component content */}
    </div>
  )
}
```

### Adding API Routes
Create in `src/app/api/` following Next.js 16 conventions:
```typescript
// src/app/api/example/route.ts
export async function GET(request: Request) {
  return Response.json({ data: 'example' })
}
```

## Best Practices

### Server vs Client Components
- **Server Components** (default): Data fetching, access to backend resources
- **Client Components** (`"use client"`): Interactivity, hooks, browser APIs

### Data Fetching
```typescript
// Server Component - direct async/await
async function Page() {
  const data = await fetch('https://api.example.com/data')
  return <div>{/* render data */}</div>
}
```

### Environment Variables
- Create `.env.local` for local development
- Use `NEXT_PUBLIC_` prefix for client-side variables
- Access with `process.env.VARIABLE_NAME`

## Testing Checklist

Before committing:
- [ ] Run `pnpm run lint` and fix issues
- [ ] Run `pnpm run build` successfully
- [ ] Test in browser at localhost:3000
- [ ] Check TypeScript errors (VSCode or `tsc --noEmit`)
- [ ] Verify responsive design (mobile, tablet, desktop)

## Deployment

### Vercel (Recommended)
```bash
pnpm add -g vercel
vercel
```

### Docker
```bash
docker build -t nextjs-app .
docker run -p 3000:3000 nextjs-app
```

## Troubleshooting

### Common Issues
- **Port 3000 in use**: Change port with `pnpm run dev -p 3001`
- **Cache issues**: Delete `.next` folder and rebuild
- **Type errors**: Run `pnpm install` to ensure types are installed
- **Hydration errors**: Check for mismatched server/client rendering

## Additional Resources

- [Next.js 16 Documentation](https://nextjs.org/docs)
- [React 19 Docs](https://react.dev)
- [Tailwind CSS v4](https://tailwindcss.com/docs)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)

## Notes for Claude Code

When working with this project:
1. Always check existing patterns before creating new ones
2. Follow the established folder structure
3. Use TypeScript types consistently
4. Test builds before committing (`pnpm run build`)
5. Prefer Server Components unless client interactivity is needed
6. Keep the AGENTS.md file updated with project-specific conventions
