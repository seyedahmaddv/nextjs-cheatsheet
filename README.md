# 📚 راهنمای جامع دستورات - Next.js Cheat Sheet

## 🔗 **فهرست سریع (کلیک کنید)**
- [📦 مدیریت پکیج](#مدیریت-پکیج)
- [🚀 اجرای پروژه](#اجرای-پروژه)
- [📁 ساختار پروژه](#ساختار-پروژه)
- [🔧 کامپوننت‌ها](#کامپوننت‌ها)
- [🔄 کانفیگ](#کانفیگ)
- [📡 Data Fetching](#data-fetching)
- [🎨 استایل‌دهی](#استایل-دهی)
- [🔌 API Routes](#api-routes)
- [🚢 Deployment](#deployment)
- [🔍 دیباگ](#دیباگ)
- [⚡ بهینه‌سازی](#بهینه-سازی)
- [📱 موبایل](#موبایل)
- [🔒 امنیت](#امنیت)
- [📊 آنالیتیکس](#آنالیتیکس)
- [🎯 Tips](#tips)

---

## 📦 **مدیریت پکیج و به‌روزرسانی** {#مدیریت-پکیج}

### **با npm**
```bash
# نصب Next.js جدید
npx create-next-app@latest [نام-پروژه]

# به‌روزرسانی به آخرین نسخه
npm install next@latest react@latest react-dom@latest

# به‌روزرسانی همه وابستگی‌ها
npm update

# بررسی نسخه‌های قدیمی
npm outdated

# نصب نسخه خاص
npm install next@13.4.0
```

### **با yarn**
```bash
# ایجاد پروژه جدید
yarn create next-app [نام-پروژه]

# به‌روزرسانی
yarn upgrade next --latest

# افزودن وابستگی
yarn add [package-name]
```

### **با pnpm**
```bash
# ایجاد پروژه
pnpm create next-app [نام-پروژه]

# به‌روزرسانی
pnpm update next --latest

# نصب با pnpm
pnpm add next
```

### **با bun** (جدیدترین)
```bash
# ایجاد پروژه
bun create next-app [نام-پروژه]

# نصب وابستگی‌ها
bun install

# اجرا با bun
bun run dev
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🚀 **اجرای پروژه** {#اجرای-پروژه}

```bash
# حالت توسعه
npm run dev
# یا
next dev

# ساخت برای تولید
npm run build
# یا
next build

# اجرای نسخه تولید
npm run start
# یا
next start

# پیش‌نمایش نسخه ساخته‌شده
npm run start

# لینت کردن کد
npm run lint
# یا
next lint

# بررسی تایپ‌اسکریپت
npm run type-check
# یا
npx tsc --noEmit
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 📁 **ساختار پروژه** {#ساختار-پروژه}

### **ساختار App Router (نسخه 13 به بعد)**
```
app/
├── layout.js/jsx/tsx      # Layout اصلی
├── page.js/jsx/tsx        # صفحه اصلی
├── loading.js/jsx/tsx     # کامپوننت Loading
├── error.js/jsx/tsx       # کامپوننت Error
├── not-found.js/jsx/tsx   # صفحه 404
├── globals.css            # استایل‌های گلوبال
├── api/                   # API Routes
│   └── route.js/ts
├── [dynamic]/             # صفحات داینامیک
│   └── page.js/ts
└── (group)/               # گروه‌بندی routes
```

### **ساختار Pages Router (نسخه‌های قدیمی‌تر)**
```
pages/
├── _app.js/jsx/tsx        # کاستومایز App
├── _document.js/jsx/tsx   # کاستومایز Document
├── index.js/jsx/tsx       # صفحه اصلی
├── api/                   # API Routes
│   └── hello.js/ts
└── [id].js/tsx            # صفحه داینامیک
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🔧 **کامپوننت‌های ویژه** {#کامپوننت-ها}

### **App Router**
```jsx
// layout.js - Layout اصلی
export default function Layout({ children }) {
  return <html><body>{children}</body></html>
}

// page.js - صفحه اصلی
export default function Page() {
  return <h1>Home Page</h1>
}

// loading.js - نمایش حین لود
export default function Loading() {
  return <div>Loading...</div>
}

// error.js - مدیریت ارور
'use client'
export default function Error({ error, reset }) {
  return <div>Error: {error.message}</div>
}
```

### **Pages Router**
```jsx
// _app.js - کاستومایز App
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}

// _document.js - کاستومایز Document
import { Html, Head, Main, NextScript } from 'next/document'
export default function Document() {
  return (
    <Html>
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🔄 **راه‌اندازی و کانفیگ** {#کانفیگ}

### **next.config.js - فایل کانفیگ**
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
  images: {
    domains: ['example.com'], // دامنه‌های مجاز برای Image
  },
  experimental: {
    appDir: true, // فعال کردن App Router
  },
  env: {
    API_URL: process.env.API_URL, // متغیرهای محیطی
  },
  // ریدایرکت و رورایت
  async redirects() {
    return [
      {
        source: '/old',
        destination: '/new',
        permanent: true,
      },
    ]
  },
  // هدرهای امنیتی
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: securityHeaders,
      },
    ]
  },
}

module.exports = nextConfig
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 📡 **Data Fetching** {#data-fetching}

### **App Router**
```jsx
// Server Components - Async/Await
export default async function Page() {
  const data = await fetch('https://api.example.com/data')
  const json = await data.json()
  
  return <div>{json.data}</div>
}

// استفاده از cache
import { cache } from 'react'
const getData = cache(async () => {
  const res = await fetch('...')
  return res.json()
})
```

### **Pages Router**
```jsx
// getServerSideProps - SSR
export async function getServerSideProps(context) {
  const res = await fetch('https://api.example.com/data')
  const data = await res.json()
  
  return {
    props: { data }
  }
}

// getStaticProps - SSG
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/data')
  const data = await res.json()
  
  return {
    props: { data },
    revalidate: 60 // ISR هر ۶۰ ثانیه
  }
}
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🎨 **استایل‌دهی** {#استایل-دهی}

```jsx
// 1. CSS Modules
import styles from './Component.module.css'
<div className={styles.container}></div>

// 2. Tailwind CSS
<div className="bg-blue-500 text-white p-4"></div>

// 3. Styled JSX
<style jsx>{`
  .container {
    background: blue;
  }
`}</style>

// 4. Styled Components
import styled from 'styled-components'
const Button = styled.button`
  background: blue;
`

// 5. Sass/SCSS
import './styles.scss'
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🔌 **API Routes** {#api-routes}

```javascript
// App Router (app/api/route.js)
export async function GET(request) {
  return new Response(JSON.stringify({ message: 'Hello' }), {
    status: 200,
    headers: { 'Content-Type': 'application/json' }
  })
}

export async function POST(request) {
  const data = await request.json()
  return new Response(JSON.stringify(data), {
    status: 201
  })
}

// Pages Router (pages/api/hello.js)
export default function handler(req, res) {
  res.status(200).json({ name: 'John Doe' })
}
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🚢 **Deployment** {#deployment}

### **Vercel (توصیه شده)**
```bash
# نصب Vercel CLI
npm i -g vercel

# دیپلوی
vercel

# دیپلوی با محیط production
vercel --prod
```

### **سایر پلتفرم‌ها**
```bash
# Dockerfile:
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🔍 **ابزارهای دیباگ** {#دیباگ}

```bash
# نمایش اطلاعات build
npx next info

# آنالیز bundle
npm run build
npx next-bundle-analyzer

# توسعه با inspector
NODE_OPTIONS='--inspect' npm run dev
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## ⚡ **بهینه‌سازی‌ها** {#بهینه-سازی}

### **Image Optimization**
```jsx
import Image from 'next/image'

<Image
  src="/profile.png"
  alt="Profile"
  width={500}
  height={500}
  priority={true} // برای LCP
  placeholder="blur"
/>
```

### **Font Optimization**
```jsx
import { Inter } from 'next/font/google'

const inter = Inter({ subsets: ['latin'] })

export default function Page() {
  return (
    <div className={inter.className}>
      My Text
    </div>
  )
}
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 📱 **Responsive & Mobile** {#موبایل}

```jsx
// استفاده از Viewport متا
import { Viewport } from 'next'

export const viewport: Viewport = {
  width: 'device-width',
  initialScale: 1,
}

// Responsive Images
<Image
  src="/hero.jpg"
  alt="Hero"
  sizes="(max-width: 768px) 100vw, 50vw"
  fill
/>
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🔒 **امنیت** {#امنیت}

```javascript
// next.config.js - هدرهای امنیتی
const securityHeaders = [
  {
    key: 'X-DNS-Prefetch-Control',
    value: 'on'
  },
  {
    key: 'Strict-Transport-Security',
    value: 'max-age=63072000; includeSubDomains; preload'
  },
  {
    key: 'X-XSS-Protection',
    value: '1; mode=block'
  }
]
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 📊 **آنالیتیکس و مانیتورینگ** {#آنالیتیکس}

```javascript
// _app.js یا app/layout.js
import { Analytics } from '@vercel/analytics/react'

export default function Layout({ children }) {
  return (
    <html>
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  )
}
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)

---

## 🎯 **Shortcuts & Tips** {#tips}

### **دستورات سریع ترمینال**
```bash
# ایجاد کامپوننت سریع
npx @next/codemod create-component Button

# مهاجرت از Pages به App Router
npx @next/codemod@canary migration

# حذف کش
rm -rf .next
rm -rf node_modules/.cache
```

### **VS Code Extensions**
- Next.js snippets
- Tailwind CSS IntelliSense
- ES7+ React/Redux/React-Native snippets

### **Environment Setup**
```bash
# پاکسازی کامل و نصب مجدد
rm -rf node_modules package-lock.json
npm cache clean --force
npm install
```

[⬆ بازگشت به فهرست](#فهرست-سریع-کلیک-کنید)


این ساختار به شما امکان می‌دهد به سرعت بین بخش‌های مختلف چیت‌شیت حرکت کنید!
