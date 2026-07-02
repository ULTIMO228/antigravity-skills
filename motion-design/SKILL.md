---
name: motion-design
description: >
  Applies motion design principles to create emotionally-driven, technically sound animations and transitions.
  Provides timing, easing, choreography, and Disney animation principles adapted for UI.
  Use when creating animations, transitions, micro-interactions, loading states, page transitions,
  scroll-triggered effects, or any motion work. Works with CSS, Framer Motion, GSAP, Lottie, Spring,
  or any animation system.
---

# Motion Design Skill

## When to Apply

Use this skill when:
- Creating UI animations (buttons, cards, modals, page transitions)
- Designing micro-interactions and feedback animations
- Building loading, success, or error states
- Animating illustrations or decorative elements
- Planning scroll-triggered or progress-based animations
- Establishing brand motion identity
- Choreographing multi-element sequences

**Decision tree:**
1. Does it serve a functional purpose (feedback, guidance)? → Timing rules for responsiveness
2. Does it express brand personality? → Motion Personality archetypes
3. Does it tell a story or guide attention? → Disney principles + choreography
4. Is this a complex multi-element scene? → 1/3 Rule + stagger patterns

---

## Quick Reference: 8-Step Checklist

Before creating any animation:

1. **Emotional target?** — joy, calm, urgency, elegance
2. **Motion Personality?** — Playful, Premium, Corporate, Energetic
3. **Primary property?** — position, scale, rotation, opacity
4. **Duration?** — see duration table below
5. **Easing family?** — entrance=decelerate, exit=accelerate
6. **Hero element?** — apply staging principles
7. **Secondary + ambient layers?** — add richness
8. **1/3 rules?** — motion distance, simultaneous elements

---

## Three Pillars (CRITICAL)

Every animation must satisfy three pillars before any technical decisions:

| Pillar | Question | Drives |
|--------|----------|--------|
| **Emotional Intent** | What should the viewer FEEL? | Easing, timing, amplitude |
| **Visual Narrative** | What's the micro-story? | Setup → Action → Resolution |
| **Motion Craft** | How do we make it believable? | Physics, secondary motion, paths |

**Three motion layers** (flat animation = missing layers):
- **Primary**: Main action the viewer follows
- **Secondary**: Supporting richness (shadows, icons shifting)
- **Ambient**: Background life (gradients, subtle pulses)

---

## Motion Personality

Select ONE archetype per project. Apply consistently.

| Archetype | Duration | Easing | Overshoot | Keywords |
|-----------|----------|--------|-----------|----------|
| **Playful** | 150-300ms | ease-out-back | 10-20% | fun, whimsical, bouncy, cute |
| **Premium** | 350-600ms | cubic-bezier(0.4,0,0.2,1) | 0% | elegant, minimal, luxury, sophisticated |
| **Corporate** | 200-400ms | cubic-bezier(0.2,0,0,1) | 0-3% | clean, professional, business, dashboard |
| **Energetic** | 100-250ms | ease-out-expo | 15-30% | dynamic, energetic, bold, exciting |

**Default**: Corporate for UI, Playful for illustrations.

**Brand Motion Identity** — define three constants:
1. **Signature easing**: One curve for 80% of animations
2. **Duration palette**: 3 durations (quick / standard / slow)
3. **Entrance pattern**: One consistent entry style

---

## Property Selection

| Effect Goal | Primary Property | Secondary Properties |
|-------------|------------------|---------------------|
| Entrance/Exit | position | opacity, scale |
| Emphasis/Attention | scale | rotation (subtle), opacity pulse |
| State Change | opacity, color | scale (press feedback) |
| Direction/Flow | position | rotation (follow path) |
| Depth/3D Feel | scale + shadow | position (parallax) |
| Loading/Progress | rotation (spinner) | scale, opacity pulse |
| Success | scale (pop) | color, rotation (checkmark draw) |
| Error/Alert | position (shake) | color, rotation (wobble) |

**Simplicity threshold**: Use the minimum properties needed. One = direct. Two = polished. Three+ = potentially overwhelming.

---

## Duration Table

| Element Type | Duration | Rationale |
|-------------|----------|-----------|
| Tooltip / micro-feedback | 80-120ms | Must feel instant |
| Button press / toggle | 120-180ms | Responsive feedback |
| Icon transition | 150-250ms | Clear state change |
| Card enter / exit | 200-350ms | Spatial awareness |
| Modal / dialog | 300-400ms | Focus shift |
| Page transition | 400-600ms | Context switch |
| Dramatic reveal | 600-1200ms | Theatrical build |

**Distance scales duration**: 100px = base. 200px = 1.3x. 400px = 1.6x.

**Enter > Exit**: Entrances 30-50% longer than exits. Users care about what appears.

**Interactive feedback**:
- Hover: <100ms
- Press: <150ms
- Release: immediate

---

## Disney's 12 Principles (Adapted for UI)

### Most Used in UI

1. **Anticipation** — Small wind-up before main action (button press → slight scale down → pop)
2. **Follow-through** — Slight overshoot past destination, settle back
3. **Ease in/out** — Never use linear for UI. Always accelerate/decelerate
4. **Staging** — Direct viewer attention. Dim surroundings when modal opens
5. **Secondary Action** — Shadow, reflection, ripple that supports primary

### Occasionally Used

6. **Squash & Stretch** — Subtle scale distortion for elasticity (max 10-15% for UI)
7. **Arcs** — Natural curved motion paths instead of straight lines
8. **Timing** — Fewer frames = snappy, more frames = graceful

### Rarely Used in UI

9. **Exaggeration** — Amplify beyond reality for effect (use sparingly)
10. **Solid Drawing** — 3D awareness in flat interfaces (parallax, depth)
11. **Appeal** — Charm and personality in motion

---

## Choreography Patterns

### Stagger

```
Element 1: ████░░░░░░░░
Element 2: ░░████░░░░░░
Element 3: ░░░░████░░░░
Element 4: ░░░░░░████░░
```

Stagger value = 40-80ms for lists, 100-150ms for cards.

### Cascade (Waterfall)

```
Header:  ████████░░░░░░
Content: ░░████████░░░░
Footer:  ░░░░████████░░
```

Each element starts after the previous reaches 30-50% completion.

### Simultaneous with Offset

```
Scale:   ████████████
Opacity: ░░████████████
Position:░░░░████████████
```

Same start time, but different durations create natural feel.

---

## 1/3 Rules

1. **Distance**: Move elements no more than 1/3 of container dimension
2. **Elements**: Animate no more than 3 things simultaneously at full complexity
3. **Duration**: Total animation sequence < 1/3 of expected user attention span
