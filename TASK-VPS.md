# TASK-VPS — Detail Page + NavBar + Final Integration
Branch: `feat/vps-detail`
Base: master (veya PC1 + PC2 merge sonrası)

## Bağımlılık
`src/lib/animations.ts`, `src/pages/HeroPage.tsx`, `src/pages/ShowcasePage.tsx` mevcut olmalı.
Eğer değilse PC1 ve PC2 task'larındaki kodları ekle.

---

## GÖREV 1: src/pages/DetailPage.tsx — Interactive detail view

Scroll-triggered reveal + animated data bars + interactive counter:

```tsx
import { motion, useInView, useSpring, useMotionValue, animate } from 'framer-motion';
import { useRef, useEffect, useState } from 'react';
import { pageVariants, stagger, fadeUp } from '../lib/animations';

function AnimatedBar({ label, value, color, delay = 0 }: { label: string; value: number; color: string; delay?: number }) {
  const ref = useRef<HTMLDivElement>(null);
  const isInView = useInView(ref, { once: true, margin: '-50px' });
  return (
    <div ref={ref} style={{ marginBottom: 20 }}>
      <div style={{ display: 'flex', justifyContent: 'space-between', marginBottom: 8, fontSize: 13 }}>
        <span style={{ color: 'var(--vz-text)' }}>{label}</span>
        <span style={{ color, fontFamily: 'monospace', fontWeight: 600 }}>{value}%</span>
      </div>
      <div style={{ height: 4, background: 'rgba(255,255,255,0.05)', borderRadius: 2, overflow: 'hidden' }}>
        <motion.div
          style={{ height: '100%', background: `linear-gradient(90deg, ${color}99, ${color})`, borderRadius: 2, originX: 0 }}
          initial={{ scaleX: 0 }}
          animate={isInView ? { scaleX: value / 100 } : { scaleX: 0 }}
          transition={{ duration: 0.8, delay, ease: [0.25, 0.46, 0.45, 0.94] }}
        />
      </div>
    </div>
  );
}

function CountUp({ to, suffix = '' }: { to: number; suffix?: string }) {
  const [val, setVal] = useState(0);
  const ref = useRef<HTMLSpanElement>(null);
  const isInView = useInView(ref, { once: true });
  useEffect(() => {
    if (!isInView) return;
    const controls = animate(0, to, {
      duration: 1.2,
      ease: 'easeOut',
      onUpdate: (v) => setVal(Math.round(v)),
    });
    return controls.stop;
  }, [isInView, to]);
  return <span ref={ref}>{val}{suffix}</span>;
}

const STATS = [
  { label: 'Spring Physics', value: 94, color: '#00F0FF' },
  { label: 'GPU Accelerated', value: 88, color: '#00FFAA' },
  { label: 'Bundle Size', value: 72, color: '#B200FF' },
  { label: 'Performance Score', value: 97, color: '#FFB800' },
];

export function DetailPage() {
  return (
    <motion.div
      className="page-wrapper"
      variants={pageVariants} initial="initial" animate="animate" exit="exit"
      style={{ minHeight: '100vh', padding: '120px 24px 80px' }}
    >
      <div style={{ maxWidth: 900, margin: '0 auto' }}>
        <motion.div variants={stagger} initial="initial" animate="animate">
          <motion.p variants={fadeUp} style={{ fontSize: 11, letterSpacing: '0.3em', color: 'var(--vz-purple)', textTransform: 'uppercase', fontFamily: 'monospace', marginBottom: 16 }}>
            ◈ Detail View
          </motion.p>

          <motion.h2 variants={fadeUp} style={{ fontSize: 'clamp(32px, 5vw, 56px)', fontWeight: 700, marginBottom: 16 }}>
            Performance Metrics
          </motion.h2>
          <motion.p variants={fadeUp} style={{ color: 'var(--vz-muted)', fontSize: 16, marginBottom: 64, lineHeight: 1.7 }}>
            All animations run at 60fps. Spring physics calculated per-frame.
          </motion.p>

          {/* Stats grid */}
          <motion.div
            variants={stagger}
            style={{ display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(180px, 1fr))', gap: 20, marginBottom: 64 }}
          >
            {[
              { label: 'Components', value: 24, suffix: '' },
              { label: 'Animations', value: 120, suffix: '+' },
              { label: 'Bundle (KB)', value: 48, suffix: '' },
              { label: 'FPS', value: 60, suffix: '' },
            ].map((stat) => (
              <motion.div
                key={stat.label}
                variants={fadeUp}
                style={{ padding: 24, borderRadius: 16, background: 'rgba(10,10,20,0.6)', border: '1px solid rgba(255,255,255,0.06)', textAlign: 'center' }}
              >
                <div style={{ fontSize: 40, fontWeight: 800, color: 'var(--vz-cyan)', fontFamily: 'monospace', lineHeight: 1 }}>
                  <CountUp to={stat.value} suffix={stat.suffix} />
                </div>
                <div style={{ fontSize: 12, color: 'var(--vz-muted)', marginTop: 8, letterSpacing: '0.05em' }}>{stat.label}</div>
              </motion.div>
            ))}
          </motion.div>

          {/* Progress bars */}
          <motion.div
            variants={fadeUp}
            style={{ padding: 32, borderRadius: 20, background: 'rgba(10,10,20,0.6)', border: '1px solid rgba(255,255,255,0.06)' }}
          >
            <div style={{ fontSize: 14, fontWeight: 600, marginBottom: 28, color: 'var(--vz-text)' }}>Benchmark Results</div>
            {STATS.map((s, i) => (
              <AnimatedBar key={s.label} {...s} delay={i * 0.1} />
            ))}
          </motion.div>
        </motion.div>
      </div>
    </motion.div>
  );
}
```

---

## GÖREV 2: src/components/NavBar.tsx — Complete NavBar

```tsx
import { motion } from 'framer-motion';
import { Link, useLocation } from 'react-router-dom';

const LINKS = [
  { to: '/', label: 'Home' },
  { to: '/showcase', label: 'Showcase' },
  { to: '/detail', label: 'Detail' },
];

export function NavBar() {
  const { pathname } = useLocation();
  return (
    <motion.nav
      initial={{ y: -60, opacity: 0 }}
      animate={{ y: 0, opacity: 1 }}
      transition={{ type: 'spring', damping: 25, stiffness: 250, delay: 0.1 }}
      style={{
        position: 'fixed', top: 0, left: 0, right: 0, zIndex: 100,
        display: 'flex', alignItems: 'center', justifyContent: 'space-between',
        padding: '0 32px', height: 64,
        background: 'rgba(2,2,5,0.85)',
        backdropFilter: 'blur(20px)',
        borderBottom: '1px solid rgba(255,255,255,0.04)',
      }}
    >
      {/* Logo */}
      <Link to="/" style={{ textDecoration: 'none' }}>
        <motion.div
          whileHover={{ opacity: 0.8 }}
          style={{ fontFamily: 'monospace', fontWeight: 700, fontSize: 15, letterSpacing: '0.2em', color: 'var(--vz-cyan)' }}
        >
          VZ·MOTION
        </motion.div>
      </Link>

      {/* Nav links */}
      <div style={{ display: 'flex', gap: 4 }}>
        {LINKS.map((link) => {
          const active = pathname === link.to;
          return (
            <Link key={link.to} to={link.to} style={{ textDecoration: 'none', position: 'relative' }}>
              <motion.div
                whileHover={{ y: -1 }}
                whileTap={{ scale: 0.96 }}
                style={{
                  padding: '8px 16px', borderRadius: 8, fontSize: 14, fontWeight: 500,
                  color: active ? 'var(--vz-cyan)' : 'var(--vz-muted)',
                  background: active ? 'rgba(0,240,255,0.06)' : 'transparent',
                  transition: 'color 0.2s, background 0.2s',
                }}
              >
                {link.label}
                {active && (
                  <motion.div
                    layoutId="nav-indicator"
                    style={{
                      position: 'absolute', bottom: 0, left: '20%', right: '20%', height: 1.5,
                      background: 'var(--vz-cyan)', borderRadius: 1,
                    }}
                    transition={{ type: 'spring', damping: 30, stiffness: 400 }}
                  />
                )}
              </motion.div>
            </Link>
          );
        })}
      </div>

      {/* Badge */}
      <motion.div
        initial={{ scale: 0 }}
        animate={{ scale: 1 }}
        transition={{ type: 'spring', delay: 0.5 }}
        style={{
          fontSize: 11, padding: '4px 12px', borderRadius: 20,
          border: '1px solid rgba(0,240,255,0.2)',
          color: 'var(--vz-cyan)', fontFamily: 'monospace',
          background: 'rgba(0,240,255,0.05)',
        }}
      >
        v1.0
      </motion.div>
    </motion.nav>
  );
}
```

## GÖREV 3: Uygulama başlatma ve doğrulama
```bash
npm run dev  # port 5174'te başlar
```
Tüm 3 sayfa açılıyor ve animasyonlar çalışıyor mu kontrol et.

```bash
npm run build  # build hatasız çıkmalı
```

## Commit
```bash
git checkout -b feat/vps-detail
git add -A
git commit -m "feat: detail page (countup, scroll bars), navbar with layoutId indicator"
git push origin feat/vps-detail
```
