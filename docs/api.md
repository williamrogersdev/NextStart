# API Routes Documentation

Learn how to create and work with API routes in this Next.js boilerplate.

## Creating API Routes

API routes are stored in `src/app/api/` and follow Next.js App Router conventions.

### Basic API Route

```typescript
// src/app/api/hello/route.ts
export async function GET() {
  return Response.json({ message: 'Hello, World!' })
}
```

### Multiple HTTP Methods

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

### Dynamic Routes

```typescript
// src/app/api/users/[id]/route.ts
export async function GET(
  request: Request,
  { params }: { params: { id: string } }
) {
  const user = await fetchUser(params.id)

  if (!user) {
    return Response.json(
      { error: 'User not found' },
      { status: 404 }
    )
  }

  return Response.json(user)
}

export async function PUT(
  request: Request,
  { params }: { params: { id: string } }
) {
  const body = await request.json()
  const user = await updateUser(params.id, body)
  return Response.json(user)
}

export async function DELETE(
  request: Request,
  { params }: { params: { id: string } }
) {
  await deleteUser(params.id)
  return Response.json({ success: true })
}
```

## Request Handling

### Reading Request Body

```typescript
export async function POST(request: Request) {
  // JSON body
  const data = await request.json()

  // Form data
  const formData = await request.formData()
  const email = formData.get('email')

  // Text body
  const text = await request.text()

  return Response.json({ received: data })
}
```

### Reading Query Parameters

```typescript
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url)
  const page = searchParams.get('page') || '1'
  const limit = searchParams.get('limit') || '10'

  const results = await fetchPaginatedData(
    parseInt(page),
    parseInt(limit)
  )

  return Response.json(results)
}
```

### Reading Headers

```typescript
export async function GET(request: Request) {
  const authorization = request.headers.get('authorization')
  const userAgent = request.headers.get('user-agent')

  if (!authorization) {
    return Response.json(
      { error: 'Unauthorized' },
      { status: 401 }
    )
  }

  // Process request...
  return Response.json({ data: 'success' })
}
```

## Response Handling

### JSON Response

```typescript
export async function GET() {
  return Response.json({ message: 'Success' })
}
```

### Custom Status Codes

```typescript
export async function POST(request: Request) {
  const body = await request.json()

  if (!body.email) {
    return Response.json(
      { error: 'Email is required' },
      { status: 400 }
    )
  }

  const user = await createUser(body)
  return Response.json(user, { status: 201 })
}
```

### Custom Headers

```typescript
export async function GET() {
  return Response.json(
    { data: 'success' },
    {
      status: 200,
      headers: {
        'Cache-Control': 'public, max-age=3600',
        'X-Custom-Header': 'value'
      }
    }
  )
}
```

### Redirects

```typescript
import { redirect } from 'next/navigation'

export async function GET() {
  redirect('/new-location')
}
```

## Error Handling

### Basic Error Handling

```typescript
export async function GET() {
  try {
    const data = await fetchData()
    return Response.json(data)
  } catch (error) {
    console.error('Error fetching data:', error)
    return Response.json(
      { error: 'Internal server error' },
      { status: 500 }
    )
  }
}
```

### Custom Error Responses

```typescript
// src/lib/api-errors.ts
export class ApiError extends Error {
  constructor(
    message: string,
    public statusCode: number = 500
  ) {
    super(message)
  }
}

// src/app/api/users/route.ts
import { ApiError } from '@/lib/api-errors'

export async function GET() {
  try {
    const users = await fetchUsers()
    return Response.json(users)
  } catch (error) {
    if (error instanceof ApiError) {
      return Response.json(
        { error: error.message },
        { status: error.statusCode }
      )
    }

    return Response.json(
      { error: 'Internal server error' },
      { status: 500 }
    )
  }
}
```

## Authentication

### Basic Authentication Check

```typescript
// src/lib/auth.ts
export async function verifyAuth(request: Request): Promise<string | null> {
  const token = request.headers.get('authorization')?.replace('Bearer ', '')

  if (!token) {
    return null
  }

  // Verify token (implement your logic)
  const userId = await verifyToken(token)
  return userId
}

// src/app/api/protected/route.ts
import { verifyAuth } from '@/lib/auth'

export async function GET(request: Request) {
  const userId = await verifyAuth(request)

  if (!userId) {
    return Response.json(
      { error: 'Unauthorized' },
      { status: 401 }
    )
  }

  const data = await fetchUserData(userId)
  return Response.json(data)
}
```

## Middleware

Create middleware in `src/middleware.ts`:

```typescript
// src/middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Check authentication
  const token = request.cookies.get('token')

  if (!token && request.nextUrl.pathname.startsWith('/api/protected')) {
    return Response.json(
      { error: 'Unauthorized' },
      { status: 401 }
    )
  }

  return NextResponse.next()
}

export const config = {
  matcher: '/api/:path*'
}
```

## CORS Configuration

```typescript
// src/app/api/public/route.ts
export async function GET(request: Request) {
  const data = { message: 'Public API' }

  return Response.json(data, {
    status: 200,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization'
    }
  })
}

export async function OPTIONS() {
  return new Response(null, {
    status: 204,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization'
    }
  })
}
```

## Rate Limiting

```typescript
// src/lib/rate-limit.ts
const rateLimitMap = new Map<string, { count: number; resetTime: number }>()

export function rateLimit(ip: string, limit: number = 10, windowMs: number = 60000) {
  const now = Date.now()
  const record = rateLimitMap.get(ip)

  if (!record || now > record.resetTime) {
    rateLimitMap.set(ip, { count: 1, resetTime: now + windowMs })
    return true
  }

  if (record.count >= limit) {
    return false
  }

  record.count++
  return true
}

// src/app/api/limited/route.ts
import { rateLimit } from '@/lib/rate-limit'

export async function GET(request: Request) {
  const ip = request.headers.get('x-forwarded-for') || 'unknown'

  if (!rateLimit(ip, 10, 60000)) {
    return Response.json(
      { error: 'Too many requests' },
      { status: 429 }
    )
  }

  return Response.json({ data: 'success' })
}
```

## Input Validation

Using Zod for validation:

```bash
pnpm add zod
```

```typescript
// src/lib/schemas.ts
import { z } from 'zod'

export const createUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2).max(50),
  age: z.number().min(18).optional()
})

// src/app/api/users/route.ts
import { createUserSchema } from '@/lib/schemas'

export async function POST(request: Request) {
  try {
    const body = await request.json()
    const validatedData = createUserSchema.parse(body)

    const user = await createUser(validatedData)
    return Response.json(user, { status: 201 })
  } catch (error) {
    if (error instanceof z.ZodError) {
      return Response.json(
        { error: 'Validation failed', details: error.errors },
        { status: 400 }
      )
    }

    return Response.json(
      { error: 'Internal server error' },
      { status: 500 }
    )
  }
}
```

## Best Practices

1. **Use TypeScript**: Type your request/response data
2. **Validate Input**: Always validate and sanitize user input
3. **Error Handling**: Implement consistent error responses
4. **Authentication**: Protect sensitive endpoints
5. **Rate Limiting**: Prevent abuse of your API
6. **CORS**: Configure properly for cross-origin requests
7. **Logging**: Log errors and important events
8. **Documentation**: Document your API endpoints
9. **Versioning**: Consider API versioning (`/api/v1/`)
10. **Testing**: Write tests for your API routes

## Testing API Routes

```typescript
// src/app/api/users/route.test.ts
import { GET, POST } from './route'

describe('/api/users', () => {
  describe('GET', () => {
    it('returns list of users', async () => {
      const response = await GET()
      const data = await response.json()

      expect(response.status).toBe(200)
      expect(Array.isArray(data)).toBe(true)
    })
  })

  describe('POST', () => {
    it('creates a new user', async () => {
      const request = new Request('http://localhost:3000/api/users', {
        method: 'POST',
        body: JSON.stringify({ email: 'test@example.com', name: 'Test' })
      })

      const response = await POST(request)
      const data = await response.json()

      expect(response.status).toBe(201)
      expect(data.email).toBe('test@example.com')
    })
  })
})
```

## Resources

- [Next.js Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)
- [Web API Reference](https://developer.mozilla.org/en-US/docs/Web/API)
- [HTTP Status Codes](https://httpstatuses.com/)
