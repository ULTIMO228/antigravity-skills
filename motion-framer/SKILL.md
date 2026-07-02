---
name: motion-framer
description: Modern animation library for React and JavaScript. Create smooth, production-ready animations with motion components, variants, gestures (hover/tap/drag), layout animations, AnimatePresence exit animations, spring physics, and scroll-based effects. Use when building interactive UI components, micro-interactions, page transitions, or complex animation sequences.
---

# Motion & Framer Motion

## Overview

Motion (formerly Framer Motion) is a production-ready animation library for React and JavaScript that enables declarative, performant animations with minimal code. It provides `motion` components that wrap HTML elements with animation superpowers, supports gesture recognition (hover, tap, drag, focus), and includes advanced features like layout animations, exit animations, and spring physics.

**When to use this skill:**
- Building interactive UI components (buttons, cards, menus)
- Creating micro-interactions and hover effects
- Implementing page transitions and route animations
- Adding scroll-based animations and parallax effects
- Animating layout changes (resizing, reordering, shared element transitions)
- Drag-and-drop interfaces
- Complex animation sequences and state-based animations
- Replacing CSS transitions with more powerful, controllable animations

**Technology:**
- **Motion** (v11+) - The modern, smaller library from Framer Motion creators
- **Framer Motion** - The full-featured predecessor (still widely used)
- React 18+ compatible, also supports Vue
- Supports TypeScript
- Works with Next.js, Vite, Remix, and all modern React frameworks

## Core Concepts

### 1. Motion Components

Convert any HTML/SVG element into an animatable component by prefixing with `motion.`:

```jsx
import { motion } from "framer-motion"

<motion.div />
<motion.button />
<motion.svg />
<motion.path />
```

Every motion component accepts animation props like `animate`, `initial`, `transition`, and gesture props like `whileHover`, `whileTap`, etc.

### 2. Animate Prop

The `animate` prop defines the target animation state. When values change, Motion automatically animates to them:

```jsx
<motion.div animate={{ x: 100 }} />

<motion.div animate={{ x: 100, opacity: 1, scale: 1.2 }} />

const [isOpen, setIsOpen] = useState(false)
<motion.div animate={{ width: isOpen ? 300 : 100 }} />
```

### 3. Initial State

```jsx
<motion.div
  initial={{ opacity: 0, y: 50 }}
  animate={{ opacity: 1, y: 0 }}
/>
```

Set `initial={false}` to disable initial animations on mount.

### 4. Transitions

```jsx
// Duration-based
<motion.div
  animate={{ x: 100 }}
  transition={{ duration: 0.5, ease: "easeInOut" }}
/>

// Spring physics
<motion.div
  animate={{ scale: 1.2 }}
  transition={{ type: "spring", stiffness: 300, damping: 20 }}
/>

// Per-property transitions
<motion.div
  animate={{ x: 100, opacity: 1 }}
  transition={{
    x: { type: "spring", stiffness: 300 },
    opacity: { duration: 0.2 }
  }}
/>
```

**Transition types:**
- `"tween"` (default) - Duration-based with easing
- `"spring"` - Physics-based spring animation
- `"inertia"` - Decelerating animation (used in drag)

### 5. Variants

```jsx
const variants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
  exit: { opacity: 0, scale: 0.9 }
}

<motion.div
  variants={variants}
  initial="hidden"
  animate="visible"
  exit="exit"
/>
```

**Variant propagation** - Children automatically inherit parent variant states:

```jsx
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1
    }
  }
}

const itemVariants = {
  hidden: { x: -20, opacity: 0 },
  visible: { x: 0, opacity: 1 }
}

<motion.ul variants={containerVariants} initial="hidden" animate="visible">
  <motion.li variants={itemVariants} />
  <motion.li variants={itemVariants} />
  <motion.li variants={itemVariants} />
</motion.ul>
```

## Common Patterns

### 1. Hover Animations

```jsx
<motion.button
  whileHover={{ scale: 1.1 }}
  transition={{ duration: 0.2 }}
>
  Hover me
</motion.button>

<motion.div
  whileHover={{
    scale: 1.05,
    backgroundColor: "#f0f0f0",
    boxShadow: "0px 10px 30px rgba(0, 0, 0, 0.2)"
  }}
>
  Hover card
</motion.div>
```

### 2. Tap/Press Animations

```jsx
<motion.button
  whileHover={{ scale: 1.1 }}
  whileTap={{ scale: 0.95, rotate: 3 }}
>
  Interactive button
</motion.button>
```

### 3. Drag Interactions

```jsx
<motion.div drag />
<motion.div drag="x" />  // Only horizontal
<motion.div
  drag
  dragConstraints={{ left: -100, right: 100, top: -100, bottom: 100 }}
/>
```

### 4. AnimatePresence (Exit Animations)

```jsx
import { AnimatePresence, motion } from "framer-motion"

<AnimatePresence mode="wait">
  {isVisible && (
    <motion.div
      key="modal"
      initial={{ opacity: 0, scale: 0.8 }}
      animate={{ opacity: 1, scale: 1 }}
      exit={{ opacity: 0, scale: 0.8 }}
      transition={{ type: "spring", stiffness: 300, damping: 25 }}
    >
      Modal content
    </motion.div>
  )}
</AnimatePresence>
```

### 5. Layout Animations

```jsx
<motion.div layout>
  {isExpanded ? <FullContent /> : <Summary />}
</motion.div>

// Shared layout animation
<motion.div layoutId="shared-element">
  Moving element
</motion.div>
```

### 6. Scroll Animations

```jsx
import { useScroll, useTransform, motion } from "framer-motion"

function ParallaxSection() {
  const { scrollYProgress } = useScroll()
  const y = useTransform(scrollYProgress, [0, 1], [0, -200])
  const opacity = useTransform(scrollYProgress, [0, 0.5, 1], [1, 0.5, 0])

  return (
    <motion.div style={{ y, opacity }}>
      Parallax content
    </motion.div>
  )
}
```

### 7. Page Transitions (Next.js)

```jsx
// app/template.tsx
"use client"
import { motion } from "framer-motion"

export default function Template({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -20 }}
      transition={{ duration: 0.3, ease: "easeInOut" }}
    >
      {children}
    </motion.div>
  )
}
```

## Performance Best Practices

- **Use `transform` and `opacity`** — hardware accelerated, no layout recalculation
- **Avoid animating `width`, `height`, `top`, `left`** — causes layout thrashing
- **Use `layout` prop** instead of animating dimensions directly
- **`will-change: transform`** on frequently animated elements
- **Lazy load motion** — `const MotionDiv = lazy(() => import("framer-motion").then(m => ({ default: m.motion.div })))`
- **Cleanup** — use `gsap.context()` or useEffect cleanup for unmounting
