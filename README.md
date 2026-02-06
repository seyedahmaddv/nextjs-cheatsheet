## Next.js Cheat Sheet 

## 🔗 فهرست سریع

* [📦 مدیریت پکیج](#مدیریت-پکیج)
* [🚀 اجرای پروژه](#اجرای-پروژه)
* [📁 ساختار پروژه](#ساختار-پروژه)
* [🔧 کامپوننت‌ها](#کامپوننتها)
* [🔄 کانفیگ](#کانفیگ)
* [📡 data fetching](#data-fetching)
* [🔌 api routes](#api-routes)
* [🎨 استایل‌دهی](#استایلدهی)
* [⚡ performance](#performance)
* [🔒 امنیت](#امنیت)
* [🚢 deployment](#deployment)
* [🧪 تست و دیباگ](#تست-و-دیباگ)
* [🎯 نکات حرفه‌ای](#نکات-حرفهای)

---

## مدیریت پکیج

```bash
# ایجاد پروژه
pnpm create next-app my-app
cd my-app

# افزودن پکیج
pnpm add axios zustand

# افزودن dev dependency
pnpm add -D eslint prettier

# به‌روزرسانی همه پکیج‌ها
pnpm update

# بررسی peer dependency issues
pnpm why react
```

📌 نکته حرفه‌ای:

* در پروژه‌های واقعی از `pnpm-lock.yaml` حتماً commit بگیر

---

## اجرای پروژه

```bash
pnpm dev        # توسعه
pnpm build      # build production
pnpm start      # اجرای build
pnpm lint       # lint
```

📌 اگر build روی سرور خطا می‌دهد:

```bash
rm -rf .next node_modules
pnpm install
```

---

## ساختار پروژه

```txt
app/
 ├─ layout.tsx
 ├─ page.tsx
 ├─ loading.tsx
 ├─ error.tsx
 ├─ not-found.tsx
 ├─ api/
 │   └─ posts/route.ts
 └─ (dashboard)/
     └─ page.tsx
```

📌 نکته:

* هر فولدر = یک route
* فایل‌های بدون `page.tsx` route نمی‌سازند

---

## کامپوننت‌ها

### Server Component (پیش‌فرض)

```tsx
export default async function Page() {
  const res = await fetch('https://api.example.com/posts')
  const posts = await res.json()

  return <pre>{JSON.stringify(posts, null, 2)}</pre>
}
```

### Client Component

```tsx
'use client'
import { useState } from 'react'

export function Counter() {
  const [count, setCount] = useState(0)
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>
}
```

---

## کانفیگ

```ts
// next.config.ts
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  reactStrictMode: true,
  images: {
    domains: ['images.unsplash.com'],
  },
}

export default nextConfig
```

---

## 📡 Data Fetching

### Static (SSG)

```ts
await fetch(url, { cache: 'force-cache' })
```

### Dynamic (SSR)

```ts
await fetch(url, { cache: 'no-store' })
```

### ISR

```ts
await fetch(url, { next: { revalidate: 60 } })
```

📌 قانون طلایی:

* داده عمومی → cache
* داده کاربر → no-store

---

## 🔌 API Routes

```ts
// app/api/posts/route.ts
import { NextResponse } from 'next/server'

export async function GET() {
  return NextResponse.json({ posts: [] })
}

export async function POST(req: Request) {
  const body = await req.json()
  return NextResponse.json(body, { status: 201 })
}
```

📌 Edge Runtime:

```ts
export const runtime = 'edge'
```

---

## استایل‌دهی

### Tailwind

```tsx
<div className="p-4 bg-zinc-900 text-white rounded-xl">Hello</div>
```

### CSS Module

```tsx
import styles from './card.module.css'
<div className={styles.card} />
```

---

## ⚡ Performance

```tsx
import dynamic from 'next/dynamic'

const Chart = dynamic(() => import('./Chart'), { ssr: false })
```

```tsx
import Image from 'next/image'
<Image src="/hero.png" alt="hero" priority />
```

---

## امنیت

```ts
// middleware.ts
import { NextResponse } from 'next/server'

export function middleware(req: Request) {
  const token = req.headers.get('authorization')
  if (!token) return NextResponse.redirect(new URL('/login', req.url))
}
```

---

## 🚢 Deployment

### Vercel

```bash
vercel --prod
```

### Docker

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY . .
RUN pnpm install && pnpm build
CMD ["pnpm","start"]
```

---

## تست و دیباگ

```bash
npx next info
pnpm lint
pnpm build --debug
```

---

## نکات حرفه ای

* از `server actions` برای فرم‌ها استفاده کن
* از `useFormStatus` برای loading فرم
* هرچه client کمتر → performance بهتر
* dependency زیاد = ریسک upgrade

---
⬆ [back up](#nextjs-cheat-sheet)
