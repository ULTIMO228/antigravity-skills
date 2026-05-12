# TypeScript Testing Reference — Vitest

## Setup
```json
// package.json
{
  "scripts": {
    "test": "vitest",
    "test:coverage": "vitest --coverage"
  }
}
```

## AAA Example
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

## Mocking
```typescript
// Mock module
vi.mock('./database', () => ({
  getConnection: vi.fn().mockResolvedValue(mockDb)
}))

// Spy on method
const spy = vi.spyOn(service, 'process')
// Restore
vi.restoreAllMocks()
```

## Tips
- `vi.fn()` for mock functions
- `vi.spyOn()` for spying existing methods
- `vi.useFakeTimers()` for time-dependent tests
- `vi.stubEnv()` for environment variables
