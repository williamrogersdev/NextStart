# Next.js AI Boilerplate

A production-ready Next.js template optimized for AI-assisted development. Clone this repository and start building your AI-powered applications immediately.

## What's Included

- **Next.js 16** - Latest version with App Router
- **React 19** - Modern React features
- **TypeScript** - Full type safety
- **Tailwind CSS v4** - Utility-first CSS framework
- **ESLint** - Code quality and consistency
- **AI-Optimized** - Includes AGENTS.md for seamless AI coding agent integration

## Quick Start

```bash
# Clone this repository
git clone <your-repo-url>
cd nextjs-boilerplate

# Install dependencies
pnpm install

# Start development server
pnpm run dev
```

Open [http://localhost:3000](http://localhost:3000) to see your application.

## Available Scripts

- `pnpm run dev` - Start development server
- `pnpm run build` - Create production build
- `pnpm run start` - Start production server
- `pnpm run lint` - Run ESLint

## Project Structure

```
nextjs-boilerplate/
├── src/
│   ├── app/            # Next.js App Router pages and layouts
│   │   ├── layout.tsx  # Root layout
│   │   ├── page.tsx    # Home page
│   │   └── globals.css # Global styles
│   ├── components/     # Reusable React components
│   ├── lib/            # Utility functions and helpers
│   └── types/          # TypeScript type definitions
├── docs/               # Project documentation
├── public/             # Static assets
├── AGENTS.md           # AI agent instructions
└── package.json        # Dependencies and scripts
```

## Working with AI Coding Agents

This project includes an `AGENTS.md` file that provides comprehensive instructions for AI coding agents like:

- Claude Code
- GitHub Copilot
- Cursor
- VS Code
- Zed
- And 20k+ other projects

The `AGENTS.md` file contains:
- Setup commands
- Code style guidelines
- Testing instructions
- Build and deployment steps
- Project-specific conventions

Simply open this project with your preferred AI coding tool, and it will automatically read the `AGENTS.md` file for context.

## Customization

1. Update `package.json` with your project name and details
2. Modify `src/app/page.tsx` to create your landing page
3. Add your components in `src/components/`
4. Add utility functions in `src/lib/`
5. Define TypeScript types in `src/types/`
6. Update `AGENTS.md` with project-specific instructions as your project evolves
7. Add project documentation in `docs/`

## Deploy

The easiest way to deploy is using [Vercel](https://vercel.com):

```bash
# Install Vercel CLI
pnpm add -g vercel

# Deploy
vercel
```

Or deploy with other platforms:
- **Netlify** - Connect your Git repository
- **AWS Amplify** - Use the Amplify Console
- **Docker** - Build and deploy as a container

## Learn More

- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://react.dev)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [AGENTS.md Format](https://agents.md)

## Contributing

This is a boilerplate template designed to be cloned and customized for your own projects. Feel free to adapt it to your needs!

## License

MIT License - feel free to use this template for any project.
