---
name: modern-web-design
description: Modern web design trends, principles, and implementation patterns for 2024-2026. Use this skill when designing websites, creating interactive experiences, implementing design systems, ensuring accessibility, or building performance-first interfaces. Triggers on tasks involving modern design trends, micro-interactions, scrollytelling, bold minimalism, cursor UX, glassmorphism, accessibility compliance, performance optimization, or design system architecture.
---

# Modern Web Design

## Overview

Modern web design in 2024-2026 emphasizes performance, accessibility, and meaningful interactions. This skill provides comprehensive guidance on current design trends, implementation patterns, and best practices for creating engaging, accessible, and performant web experiences.

## Core Design Principles

### 1. Performance-First Design

**Key Metrics**:
- Largest Contentful Paint (LCP): < 2.5s
- First Input Delay (FID): < 100ms
- Cumulative Layout Shift (CLS): < 0.1
- Interaction to Next Paint (INP): < 200ms

**Implementation Guidelines**:
- Defer non-critical animations until after page load
- Use CSS transforms/opacity for animations (GPU-accelerated)
- Implement lazy loading for images, videos, and 3D content
- Progressive enhancement: core content without JavaScript

### 2. Bold Minimalism

**Typography Scale** (Modern fluid system):
```css
--font-size-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
--font-size-sm: clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
--font-size-base: clamp(1rem, 0.9rem + 0.5vw, 1.25rem);
--font-size-lg: clamp(1.25rem, 1.1rem + 0.75vw, 1.75rem);
--font-size-xl: clamp(1.75rem, 1.5rem + 1.25vw, 2.5rem);
--font-size-2xl: clamp(2.5rem, 2rem + 2.5vw, 4rem);
--font-size-3xl: clamp(3.5rem, 2.5rem + 5vw, 6rem);
```

**Color System** (Accessibility-first):
```css
--color-primary: oklch(50% 0.2 250);
--color-accent: oklch(65% 0.25 30);
--color-neutral-50: oklch(98% 0 0);
--color-neutral-900: oklch(20% 0 0);
/* Contrast ratio: minimum 7:1 for text */
```

### 3. Micro-Interactions

**Hover States** (Desktop):
- Scale transformations (1.05-1.1x)
- Color transitions (200-300ms)
- Shadow depth changes
- Cursor transformations

**Loading States**:
- Skeleton screens (better than spinners)
- Progressive image loading (blur-up technique)
- Optimistic UI updates
- Staggered content reveals

**Interactive Feedback**:
- Button press states (scale down 0.95x)
- Toggle switches with spring physics
- Form field validation (immediate, kind feedback)
- Success/error states with motion

```jsx
<motion.button
  whileHover={{ scale: 1.05, y: -2 }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: "spring", stiffness: 400, damping: 17 }}
>
  Click me
</motion.button>
```

### 4. Scrollytelling

**Scroll-Triggered Reveals**:
- Fade-in on scroll entry (with offset)
- Slide-in from sides with stagger
- Scale + opacity transitions
- Clip-path reveals

**Scroll-Linked Animations**:
- Parallax layers (different scroll speeds)
- Horizontal scrolling sections
- Pinned sections with scrubbing animations
- 3D object rotation tied to scroll

```javascript
gsap.to(".cube", {
  scrollTrigger: {
    trigger: ".section",
    start: "top top",
    end: "bottom top",
    scrub: 1,
  },
  rotationY: 360,
  ease: "none"
});
```

### 5. Cursor UX

**Custom Cursor Shapes**:
- Circle/dot followers (delayed with easing)
- Text-based cursors ("View", "Drag", "Click")
- Blend modes for visual interest
- Scale/morph on hover

```javascript
const cursor = document.querySelector('.cursor');
let mouseX = 0, mouseY = 0;
let cursorX = 0, cursorY = 0;

document.addEventListener('mousemove', (e) => {
  mouseX = e.clientX;
  mouseY = e.clientY;
});

function updateCursor() {
  cursorX += (mouseX - cursorX) * 0.1;
  cursorY += (mouseY - cursorY) * 0.1;
  cursor.style.transform = `translate(${cursorX}px, ${cursorY}px)`;
  requestAnimationFrame(updateCursor);
}
updateCursor();
```

### 6. Glassmorphism & Depth

```css
.glass-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  border-radius: 16px;
}

/* Elevation scale */
--elevation-1: 0 1px 3px rgba(0,0,0,0.12);
--elevation-2: 0 4px 8px rgba(0,0,0,0.15);
--elevation-3: 0 8px 16px rgba(0,0,0,0.18);
--elevation-4: 0 16px 32px rgba(0,0,0,0.2);
```

### 7. Liquid Glass (2025-2026 Trend)

```css
.liquid-glass {
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.15) 0%,
    rgba(255, 255, 255, 0.05) 100%
  );
  backdrop-filter: blur(20px) saturate(200%) brightness(1.1);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 24px;
  box-shadow:
    0 4px 30px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.3);
}
```

## Common Design Patterns

### Pattern 1: Immersive Hero Section

- Full viewport height
- Subtle 3D background or animated gradient
- Large headline with fluid typography
- Smooth scroll indicator
- Parallax on scroll exit

```html
<section class="hero">
  <div id="bg-canvas"></div>
  <div class="hero__content">
    <h1 class="hero__title">Modern Design</h1>
    <p class="hero__subtitle">Performance meets beauty</p>
    <button class="hero__cta">Explore</button>
  </div>
  <div class="scroll-indicator">
    <span>Scroll</span>
  </div>
</section>
```

### Pattern 2: Horizontal Scroll Gallery

```javascript
gsap.to(".gallery__track", {
  x: () => -(document.querySelector(".gallery__track").scrollWidth - window.innerWidth),
  ease: "none",
  scrollTrigger: {
    trigger: ".gallery",
    pin: true,
    scrub: 1,
    end: () => "+=" + document.querySelector(".gallery__track").scrollWidth
  }
});
```

### Pattern 3: 3D Product Viewer

```jsx
import { Canvas } from '@react-three/fiber'
import { OrbitControls, useGLTF } from '@react-three/drei'

function ProductViewer() {
  return (
    <Canvas camera={{ position: [0, 0, 5], fov: 50 }}>
      <ambientLight intensity={0.5} />
      <spotLight position={[10, 10, 10]} />
      <ProductModel />
      <OrbitControls enableDamping autoRotate />
    </Canvas>
  )
}
```

## Accessibility Requirements

### WCAG 2.2 AA Compliance

- **Color contrast**: minimum 4.5:1 for normal text, 3:1 for large text
- **Focus indicators**: visible, high contrast, 2px minimum
- **prefers-reduced-motion**: respect system setting, provide alternatives
- **Keyboard navigation**: all interactions accessible via keyboard
- **Screen readers**: proper ARIA labels, roles, live regions

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

## 2026 Premium Landing Page Checklist

1. **Cinematic Hero** — looping video or interactive 3D, not static image
2. **Editorial Typography** — sans-serif (Geist, Inter) + serif accent
3. **Restrained Palette** — max 4 colors, dark mode as default premium
4. **Signature Mechanic** — one unique motion (horizontal scroll, character reveal, 3D transition)
5. **Buttery Scroll** — Lenis for inertia-based smoothness
6. **Scroll-Triggered Storytelling** — GSAP ScrollTrigger + Framer Motion
7. **Story-Driven Heroes** — show product workflow, not just tagline
8. **Kinetic Typography** — intentional motion for hierarchy
9. **Bento Grids** — modular content layout with hover interactions
10. **View Transitions API** — native page transitions
