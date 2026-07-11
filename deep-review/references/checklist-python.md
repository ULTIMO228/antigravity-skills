# Deep Review: Python Advanced Checklist

## Логические паттерны (красные флаги)

### Состояние и мутабельность
- [ ] Функция мутирует входной аргумент вместо возврата нового значения?
- [ ] Глобальное состояние меняется в многопоточном контексте без блокировки?
- [ ] Mutable default arguments: `def fn(data: list = [])` — классический баг
- [ ] Генераторы исчерпываются при повторном обходе (забытый `list()`)

### Асинхронность
- [ ] Blocking I/O внутри async функции (requests вместо httpx, time.sleep вместо asyncio.sleep)
- [ ] asyncio.gather без обработки частичных ошибок (return_exceptions=True нужен?)
- [ ] Задача создана через create_task, но нигде не ожидается (fire-and-forget без cancel)
- [ ] asyncio.Lock не используется для защиты shared state в coroutines
- [ ] Contextvar не сброшен после запроса (утечка между запросами в long-lived coroutine)

### Исключения и граничные случаи
- [ ] except Exception: pass — проглатывает все ошибки
- [ ] raise Exception(str(e)) — теряет traceback (нужно raise ... from e)
- [ ] Исключение поймано на слишком высоком уровне — нет деталей для debug
- [ ] finally блок без проверки — может скрыть исходное исключение
- [ ] StopIteration внутри генератора в Python 3.7+ → RuntimeError

### Типизация
- [ ] TypeVar без bounds там где нужен Protocol
- [ ] Overload декоратор не используется для полиморфных функций
- [ ] dataclass без frozen=True для value objects (непреднамеренная мутация)
- [ ] TypedDict без total=False для опциональных полей

## Производительность

### Алгоритмы
- [ ] list.index() в цикле → O(n²), заменить на set/dict
- [ ] Конкатенация строк в цикле ("+=" → join)
- [ ] sorted() вместо heapq для топ-K элементов
- [ ] deepcopy там где достаточно shallow copy
- [ ] re.compile() не вынесен из горячего пути

### Память
- [ ] Генераторы не используются для больших последовательностей
- [ ] functools.lru_cache без maxsize=N (неограниченный кеш)
- [ ] Closure захватывает большой объект — он не освобождается
- [ ] __del__ методы — непредсказуемое время вызова

### ORM/БД (SQLAlchemy / Django ORM)
- [ ] .all() без .limit() на потенциально больших таблицах
- [ ] Lazy loading в цикле (N+1) — нужен joinedload/selectinload
- [ ] Транзакция открыта на время HTTP-запроса (включая внешние вызовы)
- [ ] bulk_create / bulk_update не используется для пакетных операций
- [ ] text() запросы без bindparams — SQL-инъекция

## Безопасность Python

- [ ] pickle.loads() на недоверенных данных — RCE
- [ ] yaml.load() без Loader=yaml.SafeLoader
- [ ] subprocess с shell=True и пользовательским вводом
- [ ] os.system() вместо subprocess.run()
- [ ] tempfile.mktemp() вместо tempfile.NamedTemporaryFile()
- [ ] hashlib.md5/sha1 для паролей (нужен bcrypt/argon2)
- [ ] secrets.token_hex вместо random для криптографических нужд
- [ ] XML с lxml/ElementTree — XXE атака (disable_dtd)

## Context7 — что проверять

Запрос к context7 для Python-стека:
```
# FastAPI
resolve: "fastapi"
query: "dependency injection lifespan startup shutdown background tasks"

# SQLAlchemy 2.0
resolve: "sqlalchemy"  
query: "async session 2.0 style execute select"

# Pydantic v2
resolve: "pydantic"
query: "model_validator computed_field model_config"

# httpx
resolve: "httpx"
query: "async client timeout retry connection pool"
```

## search_web — что искать

```
"fastapi best practices 2025 performance"
"sqlalchemy 2.0 vs 1.4 migration"
"python async patterns 2025"
"[использованная библиотека] alternatives 2025"
"python [задача] fastest library benchmark"
```
