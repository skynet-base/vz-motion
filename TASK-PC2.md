# TASK-PC2 — Hero Page + Showcase Page (Animations)
Branch: `feat/pc2-pages`
Base: `feat/pc1-core` merge'ini bekle veya master'dan başla

## Bağımlılık
`src/lib/animations.ts` — pageVariants, stagger, fadeUp, scaleIn export'ları mevcut olmalı.
Eğer henüz yoksa kendisi ekle (TASK-PC1'deki içerikle).

---

## GÖREV 1: src/pages/HeroPage.tsx — Full animated hero

Full page, scroll-triggered animations:

```tsx
import { motion, useScroll, useTransform } from 'framer-motion';
import { Link } from 'react-router-dom';
import { pageVariants, stagger, fadeUp } from '../lib/animations';

export function HeroPage() {
  const { scrollYProgress } = useScroll();
  const bgY = useTransform(scrollYProgress, [0, 1], ['0%', '20%']);
  const opacity = useTransform(scrollYProgress, [0, 0.5], [1, 0]);

  return (
    <motion.div
      className="page-wrapper"
      variants={pageVariants}
      initial="initial" animate="animate" exit="exit"
      style={{ minHeight: '100vh', position: 'relative', overflow: 'hidden' }}
    >
      {/* Animated background gradient */}
      <motion.div style={{
        position: 'fixed', inset: 0, zIndex: 0,
        background: 'radial-gradient(ellipse 80% 60% at 50% -20%, rgba(0,240,255,0.12) 0%, transparent 60%), radial-gradient(ellipse 60% 40% at 80% 80%, rgba(178,0,255,0.08) 0%, transparent 60%)',
        y: bgY,
      }} />

      {/* Hero content */}
      <motion.section
        style={{ position: 'relative', zIndex: 1, display: 'flex', flexDirection: 'column', alignItems: 'center', justifyContent: 'center', minHeight: '100vh', padding: '0 24px', textAlign: 'center' }}
        variants={stagger}
        initial="initial"
        animate="animate"
      >
        <motion.div variants={fadeUp} style={{ fontSize: 11, letterSpacing: '0.3em', color: 'var(--vz-cyan)', textTransform: 'uppercase', marginBottom: 24, fontFamily: 'monospace' }}>
          ◈ Animation Showcase
        </motion.div>

        <motion.h1 variants={fadeUp} style={{ fontSize: 'clamp(48px, 8vw, 96px)', fontWeight: 800, lineHeight: 1.05, marginBottom: 24, letterSpacing: '-0.03em' }}>
          Motion at
          <motion.span
            style={{ display: 'block', background: 'linear-gradient(135deg, #00F0FF, #B200FF)', WebkitBackgroundClip: 'text', WebkitTextFillColor: 'transparent' }}
            animate={{ backgroundPosition: ['0% 50%', '100% 50%', '0% 50%'] }}
            transition={{ duration: 5, repeat: Infinity, ease: 'linear' }}
          >
            Scale.
          </motion.span>
        </motion.h1>

        <motion.p variants={fadeUp} style={{ fontSize: 18, color: 'var(--vz-muted)', maxWidth: 480, lineHeight: 1.7, marginBottom: 48 }}>
          End-to-end animations with Framer Motion — page transitions, scroll effects, spring physics, stagger.
        </motion.p>

        <motion.div variants={fadeUp} style={{ display: 'flex', gap: 16 }}>
          <motion.div whileHover={{ scale: 1.04, y: -2 }} whileTap={{ scale: 0.97 }}>
            <Link to="/showcase" style={{
              display: 'inline-block', padding: '14px 32px', borderRadius: 12,
              background: 'linear-gradient(135deg, rgba(0,240,255,0.15), rgba(178,0,255,0.1))',
              border: '1px solid rgba(0,240,255,0.3)', color: 'var(--vz-cyan)',
              textDecoration: 'none', fontWeight: 600, letterSpacing: '0.02em',
              backdropFilter: 'blur(12px)',
            }}>
              Showcase →
            </Link>
          </motion.div>
          <motion.div whileHover={{ scale: 1.04, y: -2 }} whileTap={{ scale: 0.97 }}>
            <Link to="/detail" style={{
              display: 'inline-block', padding: '14px 32px', borderRadius: 12,
              border: '1px solid rgba(255,255,255,0.08)', color: 'var(--vz-muted)',
              textDecoration: 'none', fontWeight: 500,
            }}>
              Detail View
            </Link>
          </motion.div>
        </motion.div>
      </motion.section>

      {/* Scroll hint */}
      <motion.div
        style={{ position: 'absolute', bottom: 40, left: '50%', x: '-50%', zIndex: 1 }}
        animate={{ y: [0, 8, 0] }}
        transition={{ duration: 2, repeat: Infinity }}
        initial={{ opacity: 0 }}
        whileInView={{ opacity: 1 }}
      >
        <div style={{ width: 24, height: 40, border: '1px solid rgba(0,240,255,0.2)', borderRadius: 12, display: 'flex', justifyContent: 'center', paddingTop: 6 }}>
          <motion.div
            style={{ width: 3, height: 8, background: 'var(--vz-cyan)', borderRadius: 2 }}
            animate={{ y: [0, 12, 0], opacity: [1, 0, 1] }}
            transition={{ duration: 2, repeat: Infinity }}
          />
        </div>
      </motion.div>
    </motion.div>
  );
}
```

---

## GÖREV 2: src/pages/ShowcasePage.tsx — Animated cards grid

Staggered card grid with hover effects:

```tsx
import { motion } from 'framer-motion';
import { pageVariants, stagger, fadeUp, scaleIn } from '../lib/animations';

const ITEMS = [
  { id: 1, title: 'Spring Physics', desc: 'Natural motion with damping + stiffness', color: '#00F0FF', icon: '⟳' },
  { id: 2, title: 'Page Transitions', desc: 'Smooth blur + slide between routes', color: '#B200FF', icon: '⇄' },
  { id: 3, title: 'Stagger Children', desc: 'Cascading entrance animations', color: '#00FFAA', icon: '≡' },
  { id: 4, title: 'Scroll Triggers', desc: 'useInView + viewport detection', color: '#FFB800', icon: '↕' },
  { id: 5, title: 'Gesture Control', desc: 'whileHover, whileTap, drag', color: '#FF0055', icon: '◈' },
  { id: 6, title: 'Layout Animations', desc: 'layoutId for shared element transitions', color: '#00F0FF', icon: '⬡' },
];

export function ShowcasePage() {
  return (
    <motion.div
      className="page-wrapper"
      variants={pageVariants} initial="initial" animate="animate" exit="exit"
      style={{ minHeight: '100vh', padding: '120px 24px 80px' }}
    >
      <motion.div style={{ maxWidth: 1100, margin: '0 auto' }}>
        <motion.div variants={stagger} initial="initial" animate="animate">
          <motion.p variants={fadeUp} style={{ fontSize: 11, letterSpacing: '0.3em', color: 'var(--vz-cyan)', textTransform: 'uppercase', fontFamily: 'monospace', marginBottom: 16 }}>
            ◈ Components
          </motion.p>
          <motion.h2 variants={fadeUp} style={{ fontSize: 'clamp(32px, 5vw, 56px)', fontWeight: 700, marginBottom: 64 }}>
            Animation Primitives
          </motion.h2>

          <motion.div
            style={{ display: 'grid', gridTemplateColumns: 'repeat(auto-fill, minmax(300px, 1fr))', gap: 20 }}
            variants={stagger}
          >
            {ITEMS.map((item) => (
              <motion.div
                key={item.id}
                variants={scaleIn}
                whileHover={{ y: -6, scale: 1.01 }}
                whileTap={{ scale: 0.98 }}
                style={{
                  padding: 28, borderRadius: 16,
                  background: 'rgba(10,10,20,0.6)',
                  border: `1px solid rgba(255,255,255,0.06)`,
                  backdropFilter: 'blur(20px)',
                  cursor: 'pointer',
                  transition: 'border-color 0.3s',
                  position: 'relative',
                  overflow: 'hidden',
                }}
                onHoverStart={(e: MouseEvent) => {
                  (e.currentTarget as HTMLDivElement).style.borderColor = `${item.color}44`;
                }}
                onHoverEnd={(e: MouseEvent) => {
                  (e.currentTarget as HTMLDivElement).style.borderColor = 'rgba(255,255,255,0.06)';
                }}
              >
                {/* Glow top edge */}
                <div style={{
                  position: 'absolute', top: 0, left: '20%', right: '20%', height: 1,
                  background: `linear-gradient(90deg, transparent, ${item.color}44, transparent)`,
                }} />

                <div style={{ fontSize: 28, marginBottom: 16 }}>{item.icon}</div>
                <div style={{ fontSize: 16, fontWeight: 600, marginBottom: 8, color: 'var(--vz-text)' }}>{item.title}</div>
                <div style={{ fontSize: 14, color: 'var(--vz-muted)', lineHeight: 1.6 }}>{item.desc}</div>

                {/* Color dot */}
                <motion.div
                  style={{ position: 'absolute', bottom: 20, right: 20, width: 8, height: 8, borderRadius: '50%', background: item.color }}
                  animate={{ opacity: [0.5, 1, 0.5] }}
                  transition={{ duration: 2, repeat: Infinity, delay: item.id * 0.3 }}
                />
              </motion.div>
            ))}
          </motion.div>
        </motion.div>
      </motion.div>
    </motion.div>
  );
}
```

## Commit
```bash
git checkout -b feat/pc2-pages
git add -A
git commit -m "feat: hero page + showcase page with spring animations, stagger, scroll"
git push origin feat/pc2-pages
```
