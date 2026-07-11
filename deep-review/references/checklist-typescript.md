# Deep Review: TypeScript/JavaScript Advanced Checklist

## Логические паттерны (красные флаги)

### Состояние и мутабельность
- [ ] Мутация объекта вместо создания нового (особенно в React state)
- [ ] Object.assign / spread не делает deep clone — вложенные объекты мутируются
- [ ] Array.sort() мутирует массив на месте — неожиданный side effect
- [ ] forEach не позволяет early return — нужен for...of с break/continue
- [ ] Closure захватывает переменную цикла (let vs var в for)

### Async/Await
- [ ] Floating promises — Promise не ожидается и не обрабатывается
- [ ] Promise.all без .catch — одна ошибка роняет всё
- [ ] Promise.allSettled не используется там где нужны частичные результаты
- [ ] Последовательные await в цикле вместо Promise.all (серийно вместо параллельно)
- [ ] async функция в useEffect без отмены (AbortController)
- [ ] try/catch не ловит ошибки из setTimeout/setInterval

### TypeScript специфика
- [ ] as Type вместо корректной типизации (type assertion без проверки)
- [ ] Non-null assertion (!) без реальной уверенности в non-null
- [ ] Enum вместо const enum (runtime overhead)
- [ ] any в промежуточных преобразованиях — теряется типобезопасность
- [ ] Intersection types создают невозможные типы (string & number)
- [ ] Generic параметр не используется → убрать или заменить unknown
- [ ] Отсутствие discriminated union для state machine

### React (если применимо)
- [ ] useEffect с зависимостью-объектом — ссылка меняется каждый рендер
- [ ] useState с объектом — частичное обновление перезаписывает поля
- [ ] useMemo/useCallback без реальной необходимости (преждевременная оптимизация)
- [ ] Контекст меняется на каждый рендер — все подписчики перерисовываются
- [ ] key={index} в динамических списках — баги при переупорядочивании
- [ ] Прямая мутация ref.current вместо useState для reactive данных
- [ ] Компонент > 300 строк — нарушение SRP

## Производительность

### Алгоритмы
- [ ] indexOf/includes в горячем пути → Set/Map для O(1) lookup
- [ ] Цепочка .filter().map().reduce() → один проход reduce
- [ ] Глубокое клонирование (JSON.parse(JSON.stringify())) в горячем пути
- [ ] Регулярное выражение создаётся внутри функции (не кешируется)
- [ ] Рекурсия без хвостовой оптимизации на больших входных данных

### DOM / Browser
- [ ] Прямая манипуляция DOM в React (не через ref)
- [ ] Синхронный XHR (заблокирует поток)
- [ ] Обработчик события не удаляется (утечка памяти)
- [ ] requestAnimationFrame не используется для анимаций (setTimeout вместо него)
- [ ] Отсутствие debounce/throttle на scroll/resize/input обработчиках
- [ ] Слишком большой bundle — нет lazy loading / code splitting

### Node.js / Server
- [ ] Синхронные операции FS в async контексте (fs.readFileSync)
- [ ] JSON.parse без try/catch
- [ ] Отсутствие streaming для больших ответов
- [ ] Нет connection pooling для БД
- [ ] setInterval без clearInterval при cleanup

## Безопасность TypeScript/JS

- [ ] eval() / new Function() с пользовательским вводом — RCE
- [ ] innerHTML = userInput — XSS
- [ ] dangerouslySetInnerHTML без санитизации (DOMPurify?)
- [ ] Prototype pollution через Object.assign({}, userInput)
- [ ] __proto__ / constructor в пользовательских данных
- [ ] Regex без timeout — ReDoS атака
- [ ] JWT декодируется без верификации подписи
- [ ] crypto.getRandomValues не используется для секретов (Math.random ненадёжен)
- [ ] CORS настроен на * для API с аутентификацией
- [ ] Секреты в client-side коде / env vars без VITE_/NEXT_PUBLIC_ prefix

## Context7 — что проверять

```
# Next.js
resolve: "nextjs"
query: "app router server components caching data fetching"

# React Query / TanStack
resolve: "tanstack-query"
query: "staleTime gcTime optimistic updates mutation"

# Zod
resolve: "zod"
query: "transform refine superRefine error messages"

# Prisma
resolve: "prisma"
query: "transaction nested writes select include"

# tRPC
resolve: "trpc"
query: "middleware context input output transformer"
```

## search_web — что искать

```
"[фреймворк] best practices 2025"
"[библиотека] v[N] breaking changes migration"
"react server components vs client components when to use"
"[задача] typescript library benchmark 2025"
"[использованный паттерн] alternatives modern approach"
"is [библиотека] still maintained 2025"
```
