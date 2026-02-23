# Babashka & Clojure

> **Инструменты на Clojure для автоматизации и валидации в экосистеме Rule-ROM.**

## Настройка

Инструменты на основе Clojure для автоматизации задач в экосистеме Rule-ROM.

## Требования

- **Java 11+** (для Babashka)
- **Babashka** (scripting Clojure без JVM)

## Установка

### Windows (через Scoop)

```powershell
scoop install babashka
```

### Linux

```bash
curl -s https://raw.githubusercontent.com/babashka/babashka/master/install | sudo bash
```

### macOS (через Homebrew)

```bash
brew install babashka
```

## Проверка установки

```bash
bb --version
```

## Использование в Rule-ROM

### Запуск Enforcer

```powershell
# Генерация AST
clang -Xclang -ast-dump=json -fsyntax-only test.c > ast.json

# Валидация
bb -m garden.enforcer ast.json
```

### Запуск Echo (генерация отчётов)

```powershell
bb -m garden.echo ast.json
```

## Пример EDN-контракта

```edn
{:intent :safe-free
 :entities [:ptr]
 :must-set-null true
 :description "После free() указатель должен быть установлен в NULL"}
```

## Пример garden-тегов в C

```c
// [[garden:intent(safe-free)]]
void cleanup(void* ptr) {
    free(ptr);
    ptr = NULL;
}
// [[/garden:intent]]
```

## Ресурсы

| Ресурс | Описание |
|--------|----------|
| [Babashka Docs](https://book.babashka.org/) | Официальная документация |
| [Clojure EDN](https://clojure.org/reference/reader) | Спецификация EDN |
| [Garden-Core](https://github.com/intent-garden/core) | Исходный код движка |

## Ссылки

- [Garden-Core Enforcer](enforcer.md)
- [Реестр Интентов](../registry/index.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
