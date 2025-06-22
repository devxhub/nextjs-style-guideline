# Next.js v15 Style Guide

<div align="center">

![Next.js Version](https://img.shields.io/badge/Next.js-v15-black?style=for-the-badge&logo=next.js)
![React Version](https://img.shields.io/badge/React-v19-blue?style=for-the-badge&logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-Latest-blue?style=for-the-badge&logo=typescript)
![TanStack Query](https://img.shields.io/badge/TanStack_Query-Latest-red?style=for-the-badge&logo=react-query)

**A comprehensive guide for building scalable, performant, and maintainable Next.js applications**

</div>

---

## 📋 Table of Contents

| Section                                                     | Description                                                |
| ----------------------------------------------------------- | ---------------------------------------------------------- |
| [🏗️ Project Structure](#project-structure)                  | Recommended directory organization and naming conventions  |
| [🚀 App Router Guidelines](#app-router-guidelines)          | Modern App Router patterns and best practices              |
| [⚛️ React 19 Integration](#react-19-integration)            | Leveraging React 19 features in Next.js 15                 |
| [🧩 Components & Patterns](#components--react-patterns)     | Component architecture and React patterns                  |
| [🎨 Styling Standards](#styling-standards)                  | CSS Modules, Tailwind CSS, and design system approaches    |
| [📊 Data Fetching & Caching](#data-fetching--caching)       | Server and client-side data management with TanStack Query |
| [⚡ Performance & Optimization](#performance--optimization) | Turbopack, bundle optimization, and performance techniques |
| [📝 TypeScript Guidelines](#typescript-guidelines)          | Type safety and TypeScript best practices                  |
| [🏪 State Management](#state-management)                    | React state, Context, and Zustand patterns                 |
| [🔒 Security Best Practices](#security-best-practices)      | Authentication, validation, and security measures          |
| [♿ Accessibility Standards](#accessibility-standards)      | WCAG compliance and inclusive design                       |
| [🧪 Testing Standards](#testing-standards)                  | Unit, integration, and E2E testing strategies              |
| [🚨 Error Handling](#error-handling)                        | Comprehensive error management and recovery                |
| [🌍 Internationalization](#internationalization-i18n)       | Multi-language support and localization                    |
| [⚙️ Code Formatting & Linting](#code-formatting--linting)   | Formatting, linting, and development workflow              |

---

## Project Structure

### Directory Organization

Use the App Router structure (recommended for all new projects):

```
src/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   │   └── page.tsx
│   │   └── register/
│   │       └── page.tsx
│   ├── dashboard/
│   │   ├── loading.tsx
│   │   ├── error.tsx
│   │   ├── not-found.tsx
│   │   └── page.tsx
│   ├── api/
│   │   └── users/
│   │       └── route.ts
│   ├── globals.css
│   ├── layout.tsx
│   ├── not-found.tsx
│   ├── template.tsx
│   └── page.tsx
├── components/
│   ├── ui/
│   │   ├── button.tsx
│   │   └── input.tsx
│   └── features/
│       ├── auth/
│       └── dashboard/
├── lib/
│   ├── utils.ts
│   ├── validations.ts
│   ├── auth.ts
│   └── constants.ts
├── hooks/
├── types/
├── styles/
├── middleware.ts
└── next.config.ts
```

### Naming Conventions

- **Files**: Use kebab-case for directories and camelCase for files
- **Components**: PascalCase (e.g., `UserProfile.tsx`)
- **Pages**: Use `page.tsx` for route pages
- **Layouts**: Use `layout.tsx` for layout components
- **Templates**: Use `template.tsx` for templates that recreate state
- **API Routes**: Use `route.ts` for API endpoints
- **Middleware**: Use `middleware.ts` in project root

## App Router Guidelines

### Route Organization

```typescript
// ✅ Good: Use route groups for organization
app/
├── (marketing)/
│   ├── about/page.tsx
│   └── contact/page.tsx
├── (dashboard)/
│   ├── analytics/page.tsx
│   └── settings/page.tsx
└── (auth)/
    ├── login/page.tsx
    └── register/page.tsx

// ✅ Good: Use parallel routes for complex layouts
app/
├── @sidebar/
│   └── page.tsx
├── @main/
│   └── page.tsx
└── layout.tsx

// ✅ Good: Use intercepting routes for modals
app/
├── feed/
│   └── page.tsx
├── photo/
│   └── [id]/
│       └── page.tsx
└── @modal/
    └── (..)photo/
        └── [id]/
            └── page.tsx
```

### Server Components (Default)

Prefer Server Components for better performance:

```typescript
// ✅ Good: Server Component (default)
export default async function UserList() {
  const users = await fetchUsers();

  return (
    <div>
      {users.map((user) => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
}
```

### Client Components

Use `"use client"` only when necessary:

```typescript
// ✅ Good: Client Component for interactivity
"use client";

import { useState } from "react";

export default function SearchForm() {
  const [query, setQuery] = useState("");

  return (
    <form>
      <input value={query} onChange={(e) => setQuery(e.target.value)} />
    </form>
  );
}
```

### Special Files

Always provide comprehensive special files:

```typescript
// loading.tsx
export default function Loading() {
  return (
    <div className="animate-pulse space-y-4">
      <div className="h-4 bg-gray-200 rounded w-3/4"></div>
      <div className="h-4 bg-gray-200 rounded w-1/2"></div>
    </div>
  );
}

// error.tsx
("use client");

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div className="text-center py-8">
      <h2 className="text-lg font-semibold text-red-600">
        Something went wrong!
      </h2>
      <p className="text-gray-600 mt-2">{error.message}</p>
      <button
        onClick={() => reset()}
        className="mt-4 px-4 py-2 bg-blue-600 text-white rounded"
      >
        Try again
      </button>
    </div>
  );
}

// not-found.tsx
export default function NotFound() {
  return (
    <div className="text-center py-8">
      <h2 className="text-2xl font-bold">404 - Page Not Found</h2>
      <p className="mt-2 text-gray-600">
        The page you're looking for doesn't exist.
      </p>
    </div>
  );
}
```

## React 19 Integration

### New React 19 Features

Next.js 15 supports React 19 with enhanced features:

```typescript
// ✅ Good: Use React 19 form actions
"use client";

import { useActionState } from "react";

async function createUser(prevState: any, formData: FormData) {
  const name = formData.get("name") as string;
  // Server action logic
  return { success: true, message: "User created!" };
}

export function UserForm() {
  const [state, formAction] = useActionState(createUser, null);

  return (
    <form action={formAction}>
      <input name="name" required />
      <button type="submit">Create User</button>
      {state?.message && <p>{state.message}</p>}
    </form>
  );
}

// ✅ Good: Use React 19 optimistic updates
("use client");

import { useOptimistic } from "react";

export function TodoList({ todos }: { todos: Todo[] }) {
  const [optimisticTodos, addOptimisticTodo] = useOptimistic(
    todos,
    (state, newTodo: Todo) => [...state, newTodo]
  );

  async function addTodo(formData: FormData) {
    const title = formData.get("title") as string;
    const newTodo = { id: Date.now(), title, completed: false };

    addOptimisticTodo(newTodo);
    await createTodo(newTodo);
  }

  return (
    <div>
      {optimisticTodos.map((todo) => (
        <div key={todo.id}>{todo.title}</div>
      ))}
      <form action={addTodo}>
        <input name="title" required />
        <button type="submit">Add Todo</button>
      </form>
    </div>
  );
}
```

### Enhanced Hydration Error Handling

```typescript
// ✅ Good: Better hydration error debugging with React 19
export default function ClientTime() {
  const [time, setTime] = useState<string>("");

  useEffect(() => {
    // Prevent hydration mismatch
    setTime(new Date().toLocaleTimeString());
  }, []);

  // Show nothing during SSR to prevent hydration issues
  if (!time) return <div>Loading time...</div>;

  return <div>Current time: {time}</div>;
}
```

## Components & React Patterns

### Component Structure

Follow this order for component organization:

```typescript
// ✅ Good: Consistent component structure
import { type ReactNode } from "react";
import { cn } from "@/lib/utils";

// 1. Types/Interfaces
interface ButtonProps {
  children: ReactNode;
  variant?: "primary" | "secondary" | "destructive";
  size?: "sm" | "md" | "lg";
  disabled?: boolean;
  onClick?: () => void;
}

// 2. Component
export function Button({
  children,
  variant = "primary",
  size = "md",
  disabled = false,
  onClick,
}: ButtonProps) {
  return (
    <button
      className={cn(
        "inline-flex items-center justify-center rounded-md font-medium transition-colors",
        "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2",
        {
          "bg-blue-600 text-white hover:bg-blue-700 focus-visible:ring-blue-600":
            variant === "primary",
          "bg-gray-200 text-gray-900 hover:bg-gray-300 focus-visible:ring-gray-600":
            variant === "secondary",
          "bg-red-600 text-white hover:bg-red-700 focus-visible:ring-red-600":
            variant === "destructive",
        },
        {
          "h-8 px-3 text-sm": size === "sm",
          "h-10 px-4": size === "md",
          "h-12 px-6 text-lg": size === "lg",
        },
        disabled && "opacity-50 cursor-not-allowed"
      )}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

### Server Actions Pattern

```typescript
// ✅ Good: Server actions with proper validation
"use server";

import { z } from "zod";
import { redirect } from "next/navigation";
import { revalidatePath } from "next/cache";

const createPostSchema = z.object({
  title: z.string().min(1, "Title is required"),
  content: z.string().min(10, "Content must be at least 10 characters"),
});

export async function createPost(formData: FormData) {
  const validatedFields = createPostSchema.safeParse({
    title: formData.get("title"),
    content: formData.get("content"),
  });

  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    };
  }

  try {
    await savePost(validatedFields.data);
    revalidatePath("/posts");
  } catch (error) {
    return {
      message: "Failed to create post",
    };
  }

  redirect("/posts");
}
```

## Styling Standards

### CSS Modules (Recommended)

```typescript
// UserCard.module.css
.card {
  @apply border rounded-lg p-4 shadow-sm transition-shadow;
}

.card:hover {
  @apply shadow-md;
}

.card--featured {
  @apply border-blue-500 bg-blue-50;
}

// UserCard.tsx
import styles from './UserCard.module.css';
import { cn } from '@/lib/utils';

interface UserCardProps {
  user: User;
  featured?: boolean;
}

export function UserCard({ user, featured = false }: UserCardProps) {
  return (
    <div className={cn(styles.card, featured && styles['card--featured'])}>
      <h3 className="font-semibold">{user.name}</h3>
      <p className="text-gray-600">{user.email}</p>
    </div>
  );
}
```

### Tailwind CSS Best Practices

```typescript
// ✅ Good: Use utility classes with proper grouping and responsive design
<div
  className={cn(
    // Layout
    "flex flex-col sm:flex-row items-start sm:items-center justify-between",
    // Spacing
    "p-4 gap-4 sm:p-6 sm:gap-6",
    // Appearance
    "bg-white border border-gray-200 rounded-lg shadow-sm",
    // States
    "hover:shadow-md focus-within:ring-2 focus-within:ring-blue-500",
    // Transitions
    "transition-all duration-200",
    // Dark mode
    "dark:bg-gray-800 dark:border-gray-700"
  )}
/>;

// ✅ Good: Create design system with consistent spacing
export const spacing = {
  xs: "p-2",
  sm: "p-4",
  md: "p-6",
  lg: "p-8",
  xl: "p-12",
} as const;
```

## Data Fetching & Caching

### Important Caching Changes in Next.js 15

Next.js 15 changes caching behavior - fetch requests, GET Route Handlers, and client navigations are no longer cached by default:

```typescript
// ✅ Good: Explicit caching with new defaults
export default async function PostsPage() {
  // Previously cached by default, now requires explicit caching
  const posts = await fetch("https://api.example.com/posts", {
    cache: "force-cache", // Explicit caching
    next: { revalidate: 3600 }, // Cache for 1 hour
  }).then((res) => res.json());

  return (
    <div>
      {posts.map((post) => (
        <PostCard key={post.id} post={post} />
      ))}
    </div>
  );
}

// ✅ Good: Dynamic data fetching (default behavior now)
async function getRealtimeData() {
  const res = await fetch("/api/realtime-data"); // No cache by default
  return res.json();
}

// ✅ Good: Granular cache control
async function getUser(id: string) {
  const res = await fetch(`/api/users/${id}`, {
    next: {
      revalidate: 300, // 5 minutes
      tags: [`user-${id}`], // For on-demand revalidation
    },
  });

  if (!res.ok) {
    throw new Error("Failed to fetch user");
  }

  return res.json();
}
```

### Route Handlers Caching

```typescript
// app/api/posts/route.ts
import { NextRequest, NextResponse } from "next/server";

// ✅ Good: GET routes are no longer cached by default
export async function GET(request: NextRequest) {
  const posts = await fetchPosts();

  return NextResponse.json(posts, {
    headers: {
      "Cache-Control": "max-age=3600", // Explicit caching
    },
  });
}

// ✅ Good: Use unstable_cache for expensive operations
import { unstable_cache } from "next/cache";

const getCachedPosts = unstable_cache(
  async () => {
    return await fetchPosts();
  },
  ["posts"],
  { revalidate: 3600 }
);

export async function GET() {
  const posts = await getCachedPosts();
  return NextResponse.json(posts);
}
```

### Client-Side Data Fetching

```typescript
// ✅ Good: Use React Query for client-side fetching
"use client";

import { useQuery } from "@tanstack/react-query";

const fetchUser = async (userId: string) => {
  const response = await fetch(`/api/users/${userId}`);
  if (!response.ok) {
    throw new Error("Failed to fetch user");
  }
  return response.json();
};

export function UserProfile({ userId }: { userId: string }) {
  const {
    data: user,
    error,
    isLoading,
  } = useQuery({
    queryKey: ["user", userId],
    queryFn: () => fetchUser(userId),
    refetchOnWindowFocus: false,
    refetchOnReconnect: true,
    // Optional: Add stale time to prevent unnecessary refetches
    staleTime: 5 * 60 * 1000, // 5 minutes
  });

  if (isLoading) return <UserSkeleton />;
  if (error) return <UserError error={error} />;
  if (!user) return <UserNotFound />;

  return <UserDetails user={user} />;
}
```

## Performance & Optimization

### Turbopack Integration

Next.js 15 includes stable Turbopack for development with up to 90% faster code updates:

```javascript
// next.config.ts
/** @type {import('next').NextConfig} */
const nextConfig = {
  // ✅ Good: Enable Turbopack for development (stable in v15)
  turbo: {
    rules: {
      "*.svg": {
        loaders: ["@svgr/webpack"],
        as: "*.js",
      },
    },
  },
  experimental: {
    // Enable Turbopack for builds (experimental)
    turbo: {
      loader: "turbopack",
    },
  },
};

module.exports = nextConfig;
```

### Image Optimization

```typescript
import Image from 'next/image';

// ✅ Good: Optimized images with proper sizing
<Image
  src="/hero.jpg"
  alt="Hero image"
  width={800}
  height={400}
  priority={true} // For above-the-fold images
  placeholder="blur"
  blurDataURL="data:image/jpeg;base64,..."
  className="rounded-lg"
/>

// ✅ Good: Responsive images with proper sizes
<Image
  src="/hero.jpg"
  alt="Hero image"
  fill
  className="object-cover"
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
/>
```

### Bundle Optimization

```typescript
// ✅ Good: Use dynamic imports for code splitting
import dynamic from "next/dynamic";

const DynamicChart = dynamic(() => import("./Chart"), {
  loading: () => <ChartSkeleton />,
  ssr: false, // Disable SSR if component is client-only
});

// ✅ Good: Lazy load heavy components
const LazyModal = dynamic(() => import("./Modal"), {
  loading: () => <div>Loading modal...</div>,
});

// ✅ Good: Tree-shake libraries
import { debounce } from "lodash-es"; // Not 'lodash'
import { Button } from "@/components/ui/button"; // Not entire UI library
```

### Partial Prerendering (Experimental)

```typescript
// app/dashboard/layout.tsx
export const experimental_ppr = true;

export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div className="dashboard-layout">
      <Sidebar /> {/* Static */}
      <main>
        <Suspense fallback={<Loading />}>
          {children} {/* Dynamic */}
        </Suspense>
      </main>
    </div>
  );
}
```

## TypeScript Guidelines

### Strict Configuration

```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitOverride": true
  }
}
```

### Type Definitions

```typescript
// ✅ Good: Comprehensive type definitions
interface User {
  readonly id: string;
  name: string;
  email: string;
  role: "admin" | "user" | "moderator";
  profile?: UserProfile;
  createdAt: Date;
  updatedAt: Date;
}

interface UserProfile {
  avatar?: string;
  bio: string;
  location?: string;
  website?: string;
}

// ✅ Good: Use utility types
type CreateUserInput = Omit<User, "id" | "createdAt" | "updatedAt">;
type UserUpdate = Partial<Pick<User, "name" | "email" | "profile">>;
type UserResponse = Pick<User, "id" | "name" | "email" | "role">;

// ✅ Good: Server Action types
type ActionState = {
  errors?: Record<string, string[]>;
  message?: string;
  success?: boolean;
};

type ServerAction<T = FormData> = (
  prevState: ActionState,
  formData: T
) => Promise<ActionState>;
```

## State Management

### React State (Simple Cases)

```typescript
// ✅ Good: Local state for simple components
"use client";

import { useState, useCallback } from "react";

export function Counter() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => setCount((c) => c + 1), []);
  const decrement = useCallback(() => setCount((c) => c - 1), []);

  return (
    <div>
      <span>{count}</span>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

### Context for App-Wide State

```typescript
// ✅ Good: Context for theme, auth, etc.
"use client";

import { createContext, useContext, ReactNode } from "react";

interface ThemeContextValue {
  theme: "light" | "dark";
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextValue | null>(null);

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<"light" | "dark">("light");

  const toggleTheme = useCallback(() => {
    setTheme((t) => (t === "light" ? "dark" : "light"));
  }, []);

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error("useTheme must be used within ThemeProvider");
  }
  return context;
}
```

### Zustand for Complex State

```typescript
// ✅ Good: Zustand for complex client-side state
import { create } from "zustand";
import { persist } from "zustand/middleware";

interface CartItem {
  id: string;
  name: string;
  price: number;
  quantity: number;
}

interface CartStore {
  items: CartItem[];
  total: number;
  addItem: (item: Omit<CartItem, "quantity">) => void;
  removeItem: (id: string) => void;
  updateQuantity: (id: string, quantity: number) => void;
  clearCart: () => void;
}

export const useCartStore = create<CartStore>()(
  persist(
    (set, get) => ({
      items: [],
      total: 0,
      addItem: (item) => {
        const items = get().items;
        const existingItem = items.find((i) => i.id === item.id);

        if (existingItem) {
          set((state) => ({
            items: state.items.map((i) =>
              i.id === item.id ? { ...i, quantity: i.quantity + 1 } : i
            ),
            total: state.total + item.price,
          }));
        } else {
          set((state) => ({
            items: [...state.items, { ...item, quantity: 1 }],
            total: state.total + item.price,
          }));
        }
      },
      // ... other methods
    }),
    { name: "cart-storage" }
  )
);
```

## Security Best Practices

### Authentication & Authorization

```typescript
// lib/auth.ts
import { NextRequest } from "next/server";
import { jwtVerify } from "jose";

export async function verifyToken(token: string) {
  try {
    const { payload } = await jwtVerify(
      new TextEncoder().encode(token),
      new TextEncoder().encode(process.env.JWT_SECRET!)
    );
    return payload;
  } catch (error) {
    return null;
  }
}

// middleware.ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const token = request.cookies.get("auth-token")?.value;

  if (!token && request.nextUrl.pathname.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: ["/dashboard/:path*", "/admin/:path*"],
};
```

### Input Validation

```typescript
// ✅ Good: Server-side validation with Zod
import { z } from "zod";

const userSchema = z.object({
  name: z.string().min(1).max(100),
  email: z.string().email(),
  password: z
    .string()
    .min(8)
    .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/),
});

// Server Action
export async function createUser(formData: FormData) {
  const validatedFields = userSchema.safeParse({
    name: formData.get("name"),
    email: formData.get("email"),
    password: formData.get("password"),
  });

  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    };
  }

  // Process validated data
}
```

### Environment Variables

```typescript
// lib/env.ts
import { z } from "zod";

const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  JWT_SECRET: z.string().min(32),
  NEXT_PUBLIC_API_URL: z.string().url(),
});

export const env = envSchema.parse(process.env);

// ✅ Good: Type-safe environment variables
export function getApiUrl() {
  return env.NEXT_PUBLIC_API_URL;
}
```

## Accessibility Standards

### Semantic HTML

```typescript
// ✅ Good: Proper semantic structure
export function ArticleCard({ article }: { article: Article }) {
  return (
    <article className="border rounded-lg p-4">
      <header>
        <h2 className="text-xl font-semibold">{article.title}</h2>
        <time
          dateTime={article.publishedAt.toISOString()}
          className="text-gray-600"
        >
          {article.publishedAt.toLocaleDateString()}
        </time>
      </header>
      <p>{article.excerpt}</p>
      <footer>
        <a
          href={`/articles/${article.slug}`}
          className="text-blue-600 hover:underline"
          aria-label={`Read full article: ${article.title}`}
        >
          Read more
        </a>
      </footer>
    </article>
  );
}
```

### Focus Management

```typescript
// ✅ Good: Proper focus management
"use client";

import { useRef, useEffect } from "react";

export function Modal({ isOpen, onClose, children }: ModalProps) {
  const modalRef = useRef<HTMLDivElement>(null);
  const previousActiveElement = useRef<HTMLElement | null>(null);

  useEffect(() => {
    if (isOpen) {
      previousActiveElement.current = document.activeElement as HTMLElement;
      modalRef.current?.focus();
    } else {
      previousActiveElement.current?.focus();
    }
  }, [isOpen]);

  return isOpen ? (
    <div
      ref={modalRef}
      tabIndex={-1}
      role="dialog"
      aria-modal="true"
      className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center"
      onClick={onClose}
    >
      <div
        className="bg-white rounded-lg p-6 max-w-md w-full"
        onClick={(e) => e.stopPropagation()}
      >
        {children}
      </div>
    </div>
  ) : null;
}
```

### ARIA Labels and Descriptions

```typescript
// ✅ Good: Comprehensive ARIA attributes
export function SearchForm() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);

  return (
    <form role="search">
      <label htmlFor="search-input" className="block font-medium">
        Search articles
      </label>
      <input
        id="search-input"
        type="search"
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        aria-describedby="search-help"
        aria-expanded={results.length > 0}
        aria-controls="search-results"
        className="border rounded px-3 py-2"
      />
      <div id="search-help" className="text-sm text-gray-600">
        Enter keywords to search through articles
      </div>

      {results.length > 0 && (
        <ul id="search-results" role="listbox" aria-live="polite">
          {results.map((result) => (
            <li key={result.id} role="option">
              {result.title}
            </li>
          ))}
        </ul>
      )}
    </form>
  );
}
```

## Testing Standards

### Unit Testing with Jest

```typescript
// UserCard.test.tsx
import { render, screen } from "@testing-library/react";
import { UserCard } from "./UserCard";

const mockUser = {
  id: "1",
  name: "John Doe",
  email: "john@example.com",
  role: "user" as const,
  createdAt: new Date("2024-01-01"),
  updatedAt: new Date("2024-01-01"),
};

describe("UserCard", () => {
  it("renders user information correctly", () => {
    render(<UserCard user={mockUser} />);

    expect(screen.getByText("John Doe")).toBeInTheDocument();
    expect(screen.getByText("john@example.com")).toBeInTheDocument();
    expect(screen.getByRole("article")).toBeInTheDocument();
  });

  it("handles featured state", () => {
    render(<UserCard user={mockUser} featured />);

    expect(screen.getByTestId("user-card")).toHaveClass("card--featured");
  });

  it("handles missing optional data gracefully", () => {
    const userWithoutProfile = { ...mockUser, profile: undefined };
    render(<UserCard user={userWithoutProfile} />);

    expect(screen.getByText("John Doe")).toBeInTheDocument();
  });
});
```

### Integration Testing

```typescript
// api/users.test.ts
import { POST, GET } from "./route";
import { NextRequest } from "next/server";

describe("/api/users", () => {
  beforeEach(() => {
    // Mock database or external services
    jest.clearAllMocks();
  });

  describe("POST /api/users", () => {
    it("creates user successfully", async () => {
      const request = new NextRequest("http://localhost:3000/api/users", {
        method: "POST",
        body: JSON.stringify({
          name: "John Doe",
          email: "john@example.com",
        }),
        headers: {
          "Content-Type": "application/json",
        },
      });

      const response = await POST(request);
      const data = await response.json();

      expect(response.status).toBe(201);
      expect(data).toMatchObject({
        name: "John Doe",
        email: "john@example.com",
      });
    });

    it("validates required fields", async () => {
      const request = new NextRequest("http://localhost:3000/api/users", {
        method: "POST",
        body: JSON.stringify({
          name: "", // Invalid
          email: "invalid-email", // Invalid
        }),
        headers: {
          "Content-Type": "application/json",
        },
      });

      const response = await POST(request);
      const data = await response.json();

      expect(response.status).toBe(400);
      expect(data.error).toBe("Invalid input");
      expect(data.details).toHaveProperty("name");
      expect(data.details).toHaveProperty("email");
    });
  });
});
```

### E2E Testing with Playwright

```typescript
// tests/auth.spec.ts
import { test, expect } from "@playwright/test";

test.describe("Authentication", () => {
  test("user can log in successfully", async ({ page }) => {
    await page.goto("/login");

    await page.fill("[data-testid=email-input]", "user@example.com");
    await page.fill("[data-testid=password-input]", "password123");
    await page.click("[data-testid=login-button]");

    await expect(page).toHaveURL("/dashboard");
    await expect(page.locator("[data-testid=welcome-message]")).toBeVisible();
  });

  test("shows error for invalid credentials", async ({ page }) => {
    await page.goto("/login");

    await page.fill("[data-testid=email-input]", "user@example.com");
    await page.fill("[data-testid=password-input]", "wrongpassword");
    await page.click("[data-testid=login-button]");

    await expect(page.locator("[data-testid=error-message]")).toContainText(
      "Invalid credentials"
    );
  });
});
```

### Server Action Testing

```typescript
// actions/user.test.ts
import { createUser } from "./user";

describe("createUser server action", () => {
  it("creates user with valid data", async () => {
    const formData = new FormData();
    formData.append("name", "John Doe");
    formData.append("email", "john@example.com");
    formData.append("password", "SecurePass123");

    const result = await createUser({}, formData);

    expect(result.success).toBe(true);
    expect(result.message).toBe("User created successfully");
  });

  it("returns validation errors for invalid data", async () => {
    const formData = new FormData();
    formData.append("name", ""); // Invalid
    formData.append("email", "invalid-email"); // Invalid
    formData.append("password", "123"); // Too short

    const result = await createUser({}, formData);

    expect(result.success).toBe(false);
    expect(result.errors).toHaveProperty("name");
    expect(result.errors).toHaveProperty("email");
    expect(result.errors).toHaveProperty("password");
  });
});
```

## Error Handling

### Global Error Boundaries

```typescript
// app/global-error.tsx
"use client";

import { useEffect } from "react";

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    // Log error to monitoring service
    console.error("Global error:", error);
  }, [error]);

  return (
    <html>
      <body>
        <div className="min-h-screen flex items-center justify-center bg-gray-50">
          <div className="max-w-md w-full bg-white rounded-lg shadow-lg p-6">
            <div className="flex items-center justify-center w-12 h-12 mx-auto bg-red-100 rounded-full">
              <svg
                className="w-6 h-6 text-red-600"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  strokeWidth={2}
                  d="M6 18L18 6M6 6l12 12"
                />
              </svg>
            </div>
            <h1 className="mt-4 text-xl font-semibold text-center text-gray-900">
              Something went wrong!
            </h1>
            <p className="mt-2 text-sm text-center text-gray-600">
              We apologize for the inconvenience. Please try again.
            </p>
            {process.env.NODE_ENV === "development" && (
              <details className="mt-4 p-3 bg-gray-100 rounded text-sm">
                <summary className="cursor-pointer font-medium">
                  Error Details
                </summary>
                <pre className="mt-2 whitespace-pre-wrap">{error.message}</pre>
                {error.digest && <p className="mt-1">Digest: {error.digest}</p>}
              </details>
            )}
            <button
              onClick={() => reset()}
              className="mt-6 w-full bg-blue-600 text-white py-2 px-4 rounded hover:bg-blue-700 transition-colors"
            >
              Try again
            </button>
          </div>
        </div>
      </body>
    </html>
  );
}
```

### Custom Error Classes

```typescript
// lib/errors.ts
export class AppError extends Error {
  constructor(
    message: string,
    public statusCode: number = 500,
    public code?: string,
    public cause?: Error
  ) {
    super(message);
    this.name = "AppError";
    if (cause) {
      this.cause = cause;
    }
  }
}

export class ValidationError extends AppError {
  constructor(message: string, public fields: Record<string, string[]>) {
    super(message, 400, "VALIDATION_ERROR");
    this.name = "ValidationError";
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string) {
    super(`${resource} not found`, 404, "NOT_FOUND");
    this.name = "NotFoundError";
  }
}

export class UnauthorizedError extends AppError {
  constructor(message: string = "Unauthorized") {
    super(message, 401, "UNAUTHORIZED");
    this.name = "UnauthorizedError";
  }
}

// Error handling utility
export function handleApiError(error: unknown): Response {
  if (error instanceof ValidationError) {
    return NextResponse.json(
      {
        error: error.message,
        code: error.code,
        fields: error.fields,
      },
      { status: error.statusCode }
    );
  }

  if (error instanceof AppError) {
    return NextResponse.json(
      { error: error.message, code: error.code },
      { status: error.statusCode }
    );
  }

  // Log unexpected errors
  console.error("Unexpected error:", error);

  return NextResponse.json(
    { error: "Internal server error", code: "INTERNAL_ERROR" },
    { status: 500 }
  );
}
```

### React Error Boundaries

```typescript
// components/ErrorBoundary.tsx
"use client";

import React, { Component, ReactNode } from "react";

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
  onError?: (error: Error, errorInfo: React.ErrorInfo) => void;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error("ErrorBoundary caught an error:", error, errorInfo);
    this.props.onError?.(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        this.props.fallback || (
          <div className="p-4 border border-red-200 rounded-lg bg-red-50">
            <h2 className="text-lg font-semibold text-red-800">
              Something went wrong
            </h2>
            <p className="text-red-600 mt-1">
              {this.state.error?.message || "An unexpected error occurred"}
            </p>
            <button
              onClick={() =>
                this.setState({ hasError: false, error: undefined })
              }
              className="mt-3 px-4 py-2 bg-red-600 text-white rounded hover:bg-red-700"
            >
              Try again
            </button>
          </div>
        )
      );
    }

    return this.props.children;
  }
}
```

## Internationalization (i18n)

### Next.js i18n Configuration

```typescript
// next.config.ts
/** @type {import('next').NextConfig} */
const nextConfig = {
  i18n: {
    defaultLocale: "en",
    locales: ["en", "es", "fr", "de"],
    localeDetection: true,
  },
};

export default nextConfig;
```

### Translation Utilities

```typescript
// lib/i18n.ts
import { useRouter } from "next/router";

interface Translations {
  [key: string]: string | Translations;
}

const translations: Record<string, Translations> = {
  en: {
    common: {
      loading: "Loading...",
      error: "Something went wrong",
      save: "Save",
      cancel: "Cancel",
    },
    auth: {
      login: "Log In",
      logout: "Log Out",
      email: "Email Address",
      password: "Password",
    },
  },
  es: {
    common: {
      loading: "Cargando...",
      error: "Algo salió mal",
      save: "Guardar",
      cancel: "Cancelar",
    },
    auth: {
      login: "Iniciar Sesión",
      logout: "Cerrar Sesión",
      email: "Correo Electrónico",
      password: "Contraseña",
    },
  },
};

export function useTranslation() {
  const router = useRouter();
  const locale = router.locale || "en";

  const t = (key: string): string => {
    const keys = key.split(".");
    let value: any = translations[locale];

    for (const k of keys) {
      value = value?.[k];
    }

    return typeof value === "string" ? value : key;
  };

  return { t, locale };
}
```

### Internationalized Components

```typescript
// components/LoginForm.tsx
import { useTranslation } from "@/lib/i18n";

export function LoginForm() {
  const { t } = useTranslation();

  return (
    <form className="space-y-4">
      <div>
        <label htmlFor="email" className="block text-sm font-medium">
          {t("auth.email")}
        </label>
        <input
          id="email"
          type="email"
          placeholder={t("auth.email")}
          className="mt-1 block w-full border rounded-md px-3 py-2"
        />
      </div>
      <div>
        <label htmlFor="password" className="block text-sm font-medium">
          {t("auth.password")}
        </label>
        <input
          id="password"
          type="password"
          placeholder={t("auth.password")}
          className="mt-1 block w-full border rounded-md px-3 py-2"
        />
      </div>
      <button
        type="submit"
        className="w-full bg-blue-600 text-white py-2 px-4 rounded hover:bg-blue-700"
      >
        {t("auth.login")}
      </button>
    </form>
  );
}
```

## Code Formatting & Linting

### ESLint Configuration

```json
// .eslintrc.json
{
  "extends": [
    "next/core-web-vitals",
    "@typescript-eslint/recommended",
    "@typescript-eslint/recommended-requiring-type-checking",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "rules": {
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "warn",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/no-floating-promises": "error",
    "@typescript-eslint/await-thenable": "error",
    "prefer-const": "error",
    "no-var": "error",
    "no-console": ["warn", { "allow": ["warn", "error"] }],
    "react/prop-types": "off",
    "react/react-in-jsx-scope": "off",
    "react-hooks/exhaustive-deps": "error"
  },
  "overrides": [
    {
      "files": ["**/*.test.ts", "**/*.test.tsx"],
      "rules": {
        "@typescript-eslint/no-explicit-any": "off"
      }
    }
  ]
}
```

### Prettier Configuration

```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
```

### Pre-commit Hooks

```json
// package.json
{
  "scripts": {
    "lint": "next lint",
    "lint:fix": "next lint --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "type-check": "tsc --noEmit",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:e2e": "playwright test"
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{json,md,css,scss}": ["prettier --write"]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged",
      "pre-push": "npm run type-check && npm run test"
    }
  }
}
```

### VS Code Settings

```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "typescript.preferences.importModuleSpecifier": "relative",
  "emmet.includeLanguages": {
    "typescript": "html",
    "typescriptreact": "html"
  },
  "files.associations": {
    "*.css": "tailwindcss"
  }
}
```

---

## Key Reminders for Next.js 15

### Breaking Changes & Important Updates

- **Caching Changes**: Fetch requests, GET Route Handlers, and client navigations are no longer cached by default
- **React 19 Support**: Full support for React 19 with new hooks and features
- **Turbopack Stable**: Turbopack is now stable for development builds
- **Enhanced TypeScript Support**: Better type inference and stricter checks
- **Improved Error Handling**: Better error boundaries and debugging tools

### Performance Best Practices

- Use Server Components by default for better performance
- Implement proper caching strategies with explicit cache directives
- Leverage Turbopack for faster development builds
- Use dynamic imports for code splitting
- Optimize images with Next.js Image component
- Implement proper loading states and error boundaries

### Security Considerations

- Always validate inputs on both client and server side
- Use proper authentication and authorization patterns
- Implement CSRF protection for forms
- Sanitize user inputs to prevent XSS attacks
- Use environment variables for sensitive configuration
- Keep dependencies updated and audit regularly

### Development Workflow

- Use TypeScript strict mode for better type safety
- Implement comprehensive testing (unit, integration, e2e)
- Follow accessibility best practices
- Use proper error handling and logging
- Implement internationalization from the start if needed
- Follow consistent code formatting and linting rules

This guide should be reviewed and updated quarterly to reflect new Next.js features, React updates, and team learnings. Regular team code reviews should ensure adherence to these standards.
