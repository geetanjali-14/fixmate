# FixMate Frontend — Next.js 14

The web frontend for FixMate India, built with **Next.js 14** using the App Router and TypeScript.

---

## Tech Stack

| Component         | Technology                    |
|-------------------|-------------------------------|
| Framework         | Next.js 14 (App Router)       |
| Language          | TypeScript 5                  |
| Styling           | TailwindCSS 3.4               |
| HTTP Client       | Axios                         |
| Icons             | Lucide React                  |
| Auth Storage      | js-cookie (JWT in cookies)    |
| Utility           | clsx (conditional classnames) |

---

## Prerequisites

- **Node.js** ≥ 18.x — install via [nvm](https://github.com/nvm-sh/nvm) or [nodejs.org](https://nodejs.org/)
- **npm** ≥ 9.x — comes with Node.js

---

## Installation

```bash
# 1. Navigate to the frontend directory
cd frontend

# 2. Install dependencies
npm install

# 3. Start the development server
npm run dev
```

The app will be available at **http://localhost:3000**

---

## Available Scripts

| Command          | Description                        |
|------------------|------------------------------------|
| `npm run dev`    | Start development server (port 3000) |
| `npm run build`  | Build for production               |
| `npm start`      | Start production server            |
| `npm run lint`   | Run ESLint                         |

---

## API Configuration

The backend API base URL is configured in `src/lib/api.ts`:

```typescript
const api = axios.create({
  baseURL: 'http://localhost:3001/api/v1',
});
```

JWT tokens are stored in browser cookies and automatically attached to every API request via an Axios interceptor.

> **Ensure the Rails backend is running on port 3001** before using the frontend.

---

## Project Structure

```
frontend/
├── src/
│   ├── app/                    # Next.js App Router pages
│   │   ├── page.tsx            # Landing / Home page
│   │   ├── layout.tsx          # Root layout
│   │   ├── globals.css         # Global styles
│   │   ├── login/              # Login page
│   │   │   └── page.tsx
│   │   ├── register/           # Registration page
│   │   │   └── page.tsx
│   │   ├── customer/           # Customer dashboard & pages
│   │   │   └── ...
│   │   ├── worker/             # Worker dashboard & pages
│   │   │   └── ...
│   │   └── jobs/               # Job management pages
│   │       └── ...
│   │
│   ├── components/             # Reusable React components
│   │   ├── Navbar.tsx          # Navigation bar with auth state
│   │   ├── Sidebar.tsx         # Dashboard sidebar navigation
│   │   ├── HeroSection.tsx     # Landing page hero banner
│   │   ├── WorkerCard.tsx      # Worker profile card
│   │   ├── ServiceCard.tsx     # Service listing card
│   │   ├── Button.tsx          # Reusable button component
│   │   ├── AppShell.tsx        # Layout wrapper
│   │   ├── Footer.tsx          # Site footer
│   │   ├── EmptyState.tsx      # Empty state placeholder
│   │   └── LoadingState.tsx    # Loading spinner/skeleton
│   │
│   └── lib/
│       └── api.ts              # Axios client with JWT interceptor
│
├── public/                     # Static assets
├── tailwind.config.js          # TailwindCSS configuration
├── tsconfig.json               # TypeScript configuration
├── postcss.config.js           # PostCSS configuration
├── eslint.config.mjs           # ESLint configuration
└── package.json                # Dependencies & scripts
```

---

## Design System

The TailwindCSS theme is extended with a custom design system in `tailwind.config.js`:

### Colors

| Token          | Hex       | Usage                     |
|----------------|-----------|---------------------------|
| `brand`        | `#10b981` | Primary accent (emerald)  |
| `brand-strong` | `#059669` | Hover / active states     |
| `brand-soft`   | `#34d399` | Light accent backgrounds  |
| `ink-900`      | `#0b0b10` | Darkest background        |
| `ink-800`      | `#111118` | Card backgrounds          |
| `ink-700`      | `#1a1a24` | Secondary backgrounds     |
| `ink-600`      | `#252533` | Borders and dividers      |

### Custom Utilities

- **`shadow-glow`** — Emerald glow effect: `0 0 40px rgba(16,185,129,0.2)`
- **`rounded-xl`** — `18px` border radius
- **`rounded-2xl`** — `24px` border radius
- **`font-sans`** — Primary font via CSS variable
- **`font-display`** — Display font via CSS variable

---

## Key Pages

| Route             | Description                                |
|-------------------|--------------------------------------------|
| `/`               | Landing page with hero section and service cards |
| `/login`          | User login form                            |
| `/register`       | User registration form                     |
| `/customer/*`     | Customer dashboard (search workers, manage jobs) |
| `/worker/*`       | Worker dashboard (profile, incoming jobs)   |
| `/jobs/*`         | Job detail views                           |

---

## Deployment

### Vercel (Recommended)

The easiest way to deploy is via [Vercel](https://vercel.com/):

1. Push your code to GitHub
2. Import the repository on Vercel
3. Set the root directory to `frontend`
4. Deploy

### Manual Build

```bash
# Build the production bundle
npm run build

# Start the production server
npm start
```

The production build is optimized with automatic code splitting, image optimization, and static page generation where possible.
