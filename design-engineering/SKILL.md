---
name: design-engineering
description: Комбинированный скилл дизайн-инженерии. Объединяет принципы Emil Kowalski (анимации), TasteKill (антипаттерны ИИ-дизайна) и Impeccable (типография). Применяй при создании/редизайне UI.
---

# Design Engineering Skill

Источники: Emil Kowalski (animations.dev), TasteKill (leonxlnx), Impeccable (pbakaus)

## Когда применять
- При создании нового UI компонента
- При редизайне существующего интерфейса
- При код-ревью фронтенда

## Процедура

### 1. Проверь шрифты
- ЗАБАНЕНО: Inter, Roboto, Open Sans
- Используй: `Geist`, `Satoshi`, `Outfit`, `Cabinet Grotesk`
- Serif для Dashboard/Software UI — ЗАБАНЕНО
- Монопространственный: `Geist Mono`, `JetBrains Mono`
- Body line-height: 1.6 (обязательно)
- Headlines: `clamp()` fluid sizing, tracking-tight

### 2. Проверь цвета
- ЗАБАНЕНО: AI Purple/Blue gradient, neon glows
- ЗАБАНЕНО: pure #000000 — используй off-black (#111113)
- ЗАБАНЕНО: gradient text (`background-clip: text`)
- Максимум 1 акцентный цвет, saturation < 80%
- Нейтралы: Zinc/Slate тона
- Тени: alpha ≤ 0.15, тонированные только для акцента

### 3. Проверь иконки
- ЗАБАНЕНО: emoji в коде/UI
- Используй: `@phosphor-icons/react` (Bold/Fill) или `@radix-ui/react-icons`
- Унифицируй strokeWidth по всему проекту

### 4. Проверь анимации (Emil Kowalski)
- Custom easing: `cubic-bezier(0.23, 1, 0.32, 1)` (ease-out)
- ЗАБАНЕНО: `ease-in` для UI (sluggish feeling)
- ЗАБАНЕНО: `transition: all` — указывай конкретные свойства
- ЗАБАНЕНО: `scale(0)` — начинай с `scale(0.95) + opacity: 0`
- Длительности: кнопки 100-160ms, tooltips 125-200ms, модалки 200-500ms
- Кнопки: `scale(0.97)` на `:active`, transition 160ms
- Hover gate: `@media (hover: hover) and (pointer: fine)`
- Stagger reveal: 30-80ms между элементами
- Анимируй ТОЛЬКО `transform` и `opacity`
- `prefers-reduced-motion` — обязательно

### 5. Проверь карточки/контейнеры
- Flat at rest — тень ТОЛЬКО на hover/active
- ЗАБАНЕНО: 3 одинаковых карточки в ряд
- ЗАБАНЕНО: glassmorphism как украшение
- Border-radius по весу компонента (4px-16px), не uniform
- 1px hairline border вместо тени для артикуляции

### 6. Проверь layout
- ЗАБАНЕНО: `h-screen` — используй `min-h-[100dvh]`
- ЗАБАНЕНО: complex flex math — используй CSS Grid
- Содержимое: `max-w-7xl mx-auto` или `max-w-[1400px]`
- Spacing шкала: 8/16/24/32/48/80/120px

### 7. Антипаттерны ИИ (AI Tells)
- ЗАБАНЕНО: hero-metric layout (big number + small label)
- ЗАБАНЕНО: oversized H1 headlines
- ЗАБАНЕНО: generic names (John Doe, Acme Corp)
- ЗАБАНЕНО: fake round numbers (99.99%, 50%)
- ЗАБАНЕНО: AI copywriting (Elevate, Seamless, Unleash, Next-Gen)
- ЗАБАНЕНО: broken Unsplash links — используй picsum.photos
- ЗАБАНЕНО: custom mouse cursors

## Чеклист ревью (таблица)

| Issue | Fix |
|---|---|
| `transition: all` | Указать конкретные свойства: `transition: transform 200ms ease-out` |
| `scale(0)` entry | Начинать с `scale(0.95)` + `opacity: 0` |
| `ease-in` на UI | Заменить на custom `ease-out` curve |
| Duration > 300ms на UI | Уменьшить до 150-250ms |
| Hover без media query | Добавить `@media (hover: hover) and (pointer: fine)` |
| Emoji в UI | Заменить на Phosphor/Radix иконки |
| Inter/Roboto шрифт | Заменить на Geist/Satoshi |
| Purple gradient | Заменить на один десатурированный акцент |
| `#000000` фон | Заменить на off-black (#111113) |
| Одинаковые карточки 3 в ряд | Асимметричный bento grid |
