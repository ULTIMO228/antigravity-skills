---
name: deep-context
description: >
  Используй когда задача требует сверхглубоких рассуждений: сложный баг, архитектурное
  решение, выбор между подходами. Собирает весь необходимый контекст и формирует готовый
  блок текста для копирования в окно с моделью Deep Think (Gemini 3.1 Pro Deep Think)
  или другой мощной моделью.
---

# deep-context

## Escalation Ladder (проверь сначала)
Flash → Sonnet → Opus → Deep Think. Не прыгай сразу в Deep Think если Opus не пробовал.

## Procedure

1. Formulate problem (1-2 предложения). List affected files, constraints, dep versions.
2. Collect: `.ai/PROJECT_MAP.md` + affected file contents + error traces + attempt history
3. Build ready-to-paste block:
```markdown
# Задача для Deep Think
## Контекст проекта
<стек, архитектура из PROJECT_MAP, 2-3 предложения>
## Проблема
<что не работает, конкретно>
## Ограничения
- <constraint 1>
## Затронутые файлы
### filename.ts
```lang
<content>
```
## Ошибки
```
<stack trace>
```
## Уже пробовали
1. <approach> → <result>
## Ожидаемый результат
<что должно работать>
## Вопрос
<конкретный вопрос>
```
4. Recommend model: Deep Think (debugging/algorithms) | Opus (architecture/design)
5. Tell user: «Скопируй → новое окно → модель [X] → вставь → вернись с ответом»
6. When answer received: validate → adapt to codebase → implement → test
