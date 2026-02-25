# TASK-PC1 — Core Setup & Layout
Branch: `feat/pc1-core`
Stack: Vite + React + TypeScript + Framer Motion + React Router + Tailwind

## Görev: Tailwind config + global CSS + App shell + routing

### 1. vite.config.ts — Tailwind plugin ekle
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

### 2. src/index.css — Global tokens + dark base
```css
@import "tailwindcss";

:root {
  --vz-bg: #020205;
  --vz-surface: #0A0A14;
  --vz-border: #141428;
  --vz-text: #F0F0F8;
  --vz-muted: #4A4A68;
  --vz-cyan: #00F0FF;
  --vz-purple: #B200FF;
  --vz-green: #00FFAA;
  --vz-amber: #FFB800;
}

*, *::before, *::after { box-sizing: border-box; }

body {
  margin: 0;
  background: var(--vz-bg);
  color: var(--vz-text);
  font-family: 'Inter', system-ui, sans-serif;
  -webkit-font-smoothing: antialiased;
  min-height: 100vh;
  overflow-x: hidden;
}

/* Page transition wrapper */
.page-wrapper {
  position: absolute;
  inset: 0;
  overflow-y: auto;
}

/* Scrollbar */
::-webkit-scrollbar { width: 4px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: rgba(0,240,255,0.2); border-radius: 2px; }
```

### 3. src/App.tsx — React Router + AnimatePresence page transitions
```tsx
import { BrowserRouter, Routes, Route, useLocation } from 'react-router-dom';
import { AnimatePresence } from 'framer-motion';
import { HeroPage } from './pages/HeroPage';
import { ShowcasePage } from './pages/ShowcasePage';
import { DetailPage } from './pages/DetailPage';
import { NavBar } from './components/NavBar';

function AnimatedRoutes() {
  const location = useLocation();
  return (
    <AnimatePresence mode="wait" initial={false}>
      <Routes location={location} key={location.pathname}>
        <Route path="/" element={<HeroPage />} />
        <Route path="/showcase" element={<ShowcasePage />} />
        <Route path="/detail" element={<DetailPage />} />
      </Routes>
    </AnimatePresence>
  );
}

export default function App() {
  return (
    <BrowserRouter>
      <div style={{ position: 'relative', minHeight: '100vh' }}>
        <NavBar />
        <AnimatedRoutes />
      </div>
    </BrowserRouter>
  );
}
```

### 4. src/components/NavBar.tsx — Animated navigation
- Fixed top nav, glassmorphism bg: `rgba(10,10,20,0.8) backdrop-blur`
- Logo: "VZ·MOTION" (monospace, cyan)
- 3 nav links: Home / Showcase / Detail
- Active link: cyan underline (framer-motion layoutId="nav-indicator")
- Link hover: `whileHover={{ y: -1 }}` `whileTap={{ scale: 0.97 }}`

### 5. src/lib/animations.ts — Shared animation presets
```ts
import { Variants } from 'framer-motion';

export const pageVariants: Variants = {
  initial: { opacity: 0, y: 20, filter: 'blur(4px)' },
  animate: { opacity: 1, y: 0, filter: 'blur(0px)', transition: { duration: 0.4, ease: [0.25, 0.46, 0.45, 0.94] } },
  exit:    { opacity: 0, y: -10, filter: 'blur(4px)', transition: { duration: 0.2 } },
};

export const stagger: Variants = {
  animate: { transition: { staggerChildren: 0.07 } },
};

export const fadeUp: Variants = {
  initial: { opacity: 0, y: 24 },
  animate: { opacity: 1, y: 0, transition: { type: 'spring', damping: 22, stiffness: 260 } },
};

export const scaleIn: Variants = {
  initial: { opacity: 0, scale: 0.9 },
  animate: { opacity: 1, scale: 1, transition: { type: 'spring', damping: 20, stiffness: 300 } },
};
```

### 6. src/pages/HeroPage.tsx — Placeholder (PC2'ye bırak)
```tsx
import { motion } from 'framer-motion';
import { pageVariants } from '../lib/animations';

export function HeroPage() {
  return (
    <motion.div className="page-wrapper" variants={pageVariants} initial="initial" animate="animate" exit="exit">
      {/* PC2 bu sayfayı dolduracak */}
      <div style={{ padding: '120px 24px', textAlign: 'center', color: 'var(--vz-muted)' }}>
        HeroPage — PC2 tarafından tamamlanacak
      </div>
    </motion.div>
  );
}
```

### 7. src/pages/ShowcasePage.tsx — Placeholder (PC2'ye bırak)
Aynı pattern ile placeholder.

### 8. src/pages/DetailPage.tsx — Placeholder (VPS'e bırak)
Aynı pattern ile placeholder.

## Commit
```bash
git checkout -b feat/pc1-core
git add -A
git commit -m "feat: core setup — routing, tailwind, tokens, nav, animation presets"
git push origin feat/pc1-core
```
