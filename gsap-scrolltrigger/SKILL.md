---
name: gsap-scrolltrigger
description: Comprehensive skill for GSAP (GreenSock Animation Platform) and ScrollTrigger plugin. Use this skill when creating web animations, scroll-driven experiences, timelines, tweens, scroll-triggered animations, pinning, scrubbing, parallax effects, or animating DOM elements, SVG, Canvas, WebGL, or Three.js. Triggers on tasks involving GSAP, ScrollTrigger, smooth animations, scroll effects, or animation sequencing.
---

# GSAP & ScrollTrigger Development

## Overview

GSAP (GreenSock Animation Platform) is the industry-leading JavaScript animation library for creating high-performance, production-quality animations. ScrollTrigger is GSAP's powerful plugin for scroll-driven animations. Together, they enable everything from simple UI transitions to complex scroll-based storytelling experiences.

## Core Concepts

### The Basics: Tweens

A **tween** is a single animation from point A to point B.

```javascript
// Animate TO a state (from current)
gsap.to(".box", {
  x: 200,
  rotation: 360,
  duration: 1,
  ease: "power2.inOut"
});

// Animate FROM a state (to current)
gsap.from(".box", {
  opacity: 0,
  y: -50,
  duration: 0.8
});

// Animate FROM-TO (define both start and end)
gsap.fromTo(".box",
  { opacity: 0, scale: 0.5 },
  { opacity: 1, scale: 1, duration: 1 }
);
```

### Timelines: Sequencing Animations

**Timelines** orchestrate multiple tweens in sequence or overlap.

```javascript
const tl = gsap.timeline();

tl.to(".box1", { x: 100, duration: 1 })
  .to(".box2", { y: 100, duration: 1 })
  .to(".box3", { rotation: 360, duration: 1 });

// With labels
tl.addLabel("start")
  .to(".hero", { opacity: 1, duration: 1 })
  .addLabel("reveal")
  .to(".content", { y: 0, duration: 0.8 }, "reveal")
  .to(".cta", { scale: 1, duration: 0.5 }, "reveal+=0.5");
```

### Position Parameter (Timeline Timing)

```javascript
const tl = gsap.timeline();

tl.to(".box1", { x: 100 })
  .to(".box2", { y: 100 }, 0);          // Starts at 0 seconds

tl.to(".box1", { x: 100, duration: 2 })
  .to(".box2", { y: 100 }, "-=1");      // 1 second before box1 ends
  .to(".box3", { rotation: 360 }, "+=0.5"); // 0.5s after box2 finishes

tl.to(".box1", { x: 100 }, 2.5);        // At 2.5 seconds
```

## ScrollTrigger Fundamentals

### Basic Scroll Animation

```javascript
gsap.registerPlugin(ScrollTrigger);

gsap.to(".box", {
  x: 500,
  scrollTrigger: {
    trigger: ".box",
    start: "top center",
    end: "bottom center",
    markers: true,       // Development only
    scrub: true,         // Links animation to scrollbar
    toggleActions: "play none none reverse"
  }
});
```

### Start & End Positions

Format: `"[trigger position] [viewport position]"`

```javascript
start: "top top"       // Trigger top hits viewport top
start: "top center"    // Trigger top hits viewport center (default)
start: "top bottom"    // Trigger top hits viewport bottom
start: "top top+=100"  // 100px below viewport top
start: "top 80%"       // 80% down the viewport
end: "+=500"           // 500px after start position
```

### Scrubbing (Scroll-Synced Animation)

```javascript
scrub: true   // Direct link to scrollbar (immediate)
scrub: 1      // Takes 1 second to "catch up" to scrollbar
scrub: 0.5    // Faster, tighter feel
```

### Toggle Actions

```javascript
toggleActions: "play pause resume reset"
// onEnter | onLeave | onEnterBack | onLeaveBack
// Actions: play, pause, resume, restart, reset, complete, reverse, none

// Common patterns:
toggleActions: "play none none none"        // Play once
toggleActions: "play none none reverse"     // Play forward, reverse back
toggleActions: "play complete reverse reset" // Full control
```

## Common Patterns

### 1. Fade In On Scroll

```javascript
gsap.from(".fade-in", {
  opacity: 0,
  y: 50,
  duration: 1,
  scrollTrigger: {
    trigger: ".fade-in",
    start: "top 80%",
    end: "top 50%",
    scrub: 1,
    once: true
  }
});
```

### 2. Pin Element While Scrolling

```javascript
ScrollTrigger.create({
  trigger: ".panel",
  start: "top top",
  end: "+=500",
  pin: true,
  pinSpacing: true
});
```

### 3. Horizontal Scroll Section

```javascript
const sections = gsap.utils.toArray(".panel");

gsap.to(sections, {
  xPercent: -100 * (sections.length - 1),
  ease: "none",
  scrollTrigger: {
    trigger: ".container",
    pin: true,
    scrub: 1,
    end: () => "+=" + document.querySelector(".container").offsetWidth
  }
});
```

### 4. Parallax Effect

```javascript
gsap.to(".bg", {
  y: 200,
  ease: "none",
  scrollTrigger: {
    trigger: ".section",
    start: "top bottom",
    end: "bottom top",
    scrub: true
  }
});
```

### 5. Staggered Entrance

```javascript
gsap.from(".card", {
  opacity: 0,
  y: 60,
  duration: 0.8,
  stagger: 0.15,
  ease: "power3.out",
  scrollTrigger: {
    trigger: ".cards-grid",
    start: "top 75%"
  }
});
```

### 6. Text Reveal (Character by Character)

```javascript
import { SplitText } from "gsap/SplitText";
gsap.registerPlugin(SplitText);

const split = new SplitText(".headline", { type: "chars,words" });

gsap.from(split.chars, {
  opacity: 0,
  y: 20,
  rotationX: -90,
  stagger: 0.02,
  duration: 0.5,
  ease: "back.out(1.7)",
  scrollTrigger: {
    trigger: ".headline",
    start: "top 80%"
  }
});
```

## Easing Reference

| Ease | Feel | Use Case |
|------|------|----------|
| `power1.out` | Gentle deceleration | Subtle entrances |
| `power2.out` | Smooth deceleration | Most UI animations |
| `power3.out` | Strong deceleration | Dramatic entrances |
| `power4.out` | Very strong | Hero reveals |
| `back.out(1.7)` | Overshoot | Playful bounces |
| `elastic.out(1, 0.3)` | Springy | Attention-grabbers |
| `expo.out` | Ultra-fast start | Snappy interactions |
| `none` | Linear | Scroll-synced |

## Integration with Lenis (Smooth Scroll)

```javascript
import Lenis from 'lenis';

const lenis = new Lenis();

lenis.on('scroll', ScrollTrigger.update);

gsap.ticker.add((time) => {
  lenis.raf(time * 1000);
});
gsap.ticker.lagSmoothing(0);
```

## Performance Tips

- **Use `will-change: transform`** on animated elements
- **Prefer transforms** (`x`, `y`, `scale`, `rotation`) over layout properties (`width`, `top`, `left`)
- **Use `ScrollTrigger.batch()`** for many similar animations
- **Kill ScrollTriggers** on component unmount: `ScrollTrigger.getAll().forEach(t => t.kill())`
- **Use `gsap.context()`** for React/Vue cleanup
