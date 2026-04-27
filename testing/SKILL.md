---
name: testing
description: >
  Используй когда реализована бизнес-логика, завершена фича, или пользователь просит
  написать тесты. Определяет тип тестов (unit/integration/e2e), покрывает happy path,
  edge cases и error cases по паттерну AAA. Запускает тесты и проверяет результат.
---

# testing

## Procedure

1. **Type**: isolated fn → unit | API/module interaction → integration | user journey → e2e. Priority: unit > integration > e2e.
2. **Tools**: check package.json for existing runner. Defaults: Vitest (Vite) | Jest (other) | Playwright (e2e). No runner → propose install.
3. **Placement**: follow existing convention (`__tests__/` | `*.test.ts` | `tests/`). No convention → `src/__tests__/<module>.test.ts`
4. **Write** by AAA pattern. Min 3 tests/function: happy + edge + error. Names на русском.

```typescript
import { describe, it, expect, vi } from 'vitest'

const mockRepo = { findByEmail: vi.fn() }

describe('AuthService', () => {
  it('должен вернуть токен при валидных данных', async () => {
    // Arrange
    mockRepo.findByEmail.mockResolvedValue({ id: '1', email: 'a@b.com' })
    // Act
    const result = await svc.login({ email: 'a@b.com', password: 'ok' })
    // Assert
    expect(result.token).toBeDefined()
  })

  it('должен обработать пустой email', async () => {
    await expect(svc.login({ email: '', password: 'x' }))
      .rejects.toThrow('Email обязателен')
  })

  it('должен выбросить ошибку при неверном пароле', async () => {
    mockRepo.findByEmail.mockResolvedValue({ id: '1' })
    await expect(svc.login({ email: 'a@b.com', password: 'wrong' }))
      .rejects.toThrow('Неверные учётные данные')
  })
})
```

5. **Rules**: test behavior not implementation | each test independent | mock external deps only | don't chase 100% coverage — focus business logic ≥ 80%
6. **Run**: `pnpm test`. All must pass. Coverage optional: `pnpm test --coverage`

→ After: `code-review` or `git-commit`
