# Getting Started

This guide will help you get up and running with the Next.js AI Boilerplate.

## Prerequisites

- Node.js 18.x or higher
- npm, pnpm, or yarn
- Git

## Installation

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd nextjs-boilerplate
```

### 2. Install Dependencies

```bash
npm install
# or
pnpm install
# or
yarn install
```

### 3. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Project Structure

```
nextjs-boilerplate/
├── src/
│   ├── app/            # Next.js App Router pages
│   ├── components/     # React components
│   ├── lib/            # Utility functions
│   └── types/          # TypeScript types
├── docs/               # Documentation
├── public/             # Static files
└── AGENTS.md           # AI agent instructions
```

## First Steps

### 1. Customize the Home Page

Edit `src/app/page.tsx`:

```typescript
export default function Home() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-center p-24">
      <h1 className="text-4xl font-bold">Welcome to Your App</h1>
    </main>
  )
}
```

### 2. Create Your First Component

Create `src/components/ui/Button.tsx`:

```typescript
'use client'

interface ButtonProps {
  children: React.ReactNode
  onClick?: () => void
}

export default function Button({ children, onClick }: ButtonProps) {
  return (
    <button
      onClick={onClick}
      className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
    >
      {children}
    </button>
  )
}
```

### 3. Add a New Page

Create `src/app/about/page.tsx`:

```typescript
export default function AboutPage() {
  return (
    <div className="container mx-auto p-8">
      <h1 className="text-3xl font-bold mb-4">About Us</h1>
      <p>Welcome to our about page!</p>
    </div>
  )
}
```

Visit [http://localhost:3000/about](http://localhost:3000/about) to see your new page.

### 4. Add Environment Variables

Create `.env.local`:

```bash
# Public variables (accessible in browser)
NEXT_PUBLIC_API_URL=https://api.example.com

# Private variables (server-side only)
DATABASE_URL=postgresql://...
API_SECRET=your-secret-key
```

Access them in your code:

```typescript
const apiUrl = process.env.NEXT_PUBLIC_API_URL
const dbUrl = process.env.DATABASE_URL // Server-side only
```

## Working with AI Coding Agents

This project is optimized for AI-assisted development:

1. Open the project in your AI coding tool (Claude Code, Cursor, etc.)
2. The tool will automatically read `AGENTS.md` for context
3. Ask the AI to help with:
   - Creating new features
   - Refactoring code
   - Adding tests
   - Fixing bugs

## Next Steps

- Read the [Architecture Documentation](./architecture.md)
- Explore the [Component Guide](./components.md)
- Learn about [API Routes](./api.md)
- Review the [Styling Guide](./styling.md)
- Check out [Deployment Options](./deployment.md)

## Common Commands

```bash
# Development
npm run dev          # Start dev server
npm run build        # Build for production
npm run start        # Start production server
npm run lint         # Run ESLint

# Type checking
npx tsc --noEmit     # Check TypeScript types
```

## Troubleshooting

### Port 3000 is already in use

```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or use a different port
npm run dev -- -p 3001
```

### Module not found errors

```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
```

### Tailwind styles not working

```bash
# Clear Next.js cache
rm -rf .next
npm run dev
```

## Getting Help

- Check the [FAQ](./faq.md) (if available)
- Review [AGENTS.md](../AGENTS.md) for AI agent instructions
- Visit [Next.js Documentation](https://nextjs.org/docs)
