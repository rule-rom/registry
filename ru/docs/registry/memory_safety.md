# Безопасность памяти (C-SAFE)

**Intent ID:** `:safe-pointer-lifecycle`  
**Статус:** ✅ Public  
**Версия:** 1.0

## Описание

Контракт обеспечивает безопасный жизненный цикл указателей в C: после вызова `free()` указатель **обязан** быть установлен в `NULL`.

## EDN Спецификация

```edn
{:intent :safe-pointer-lifecycle
 :rules [
   {:match :call :name "free"}
   {:ensure-next :assignment :value "NULL"}
   {:forbid-after :read-access}
 ]
 :tags ["[[garden:free]]"]}
```

## Полная спецификация (Resource Guard)

```edn
{:intent :resource-guard
 :entities {:ptr {:type "void*" :role :managed-handle}}
 :invariants [
   ;; Правило: после free указатель ОБЯЗАН стать NULL
   {:op :call :fn "free" :args [:ptr]
    :must-follow {:op :assign :target :ptr :value "NULL"}}

   ;; Правило: запрет на чтение после освобождения
   {:op :access :target :ptr :mode :read
    :forbidden-after {:op :call :fn "free"}}]
 :tags {:wrap "// [[garden:scoped]]"}}
```

## Пример использования

### ✅ Валидный код

```c
// [[garden:intent(safe-pointer-lifecycle)]]
void cleanup_resource(void* ptr) {
    if (ptr != NULL) {
        free(ptr);
        ptr = NULL;  // Обязательно!
    }
}
// [[/garden:intent]]
```

### ❌ Нарушение

```c
// [[garden:intent(safe-pointer-lifecycle)]]
void unsafe_cleanup(void* ptr) {
    free(ptr);
    // ОШИБКА: ptr не установлен в NULL
    // Нарушение: must-follow {:op :assign :target :ptr :value "NULL"}
}
// [[/garden:intent]]
```

## Правила валидации

| Правило | Описание | Статус |
|---------|----------|--------|
| `must-follow` | После `free()` следует `ptr = NULL` | Обязательно |
| `forbidden-after` | Запрет чтения после `free()` | Обязательно |
| `match` | Матчинг вызова `free()` | Обязательно |

## Инструменты

- **Enforcer:** Babashka-скрипт для валидации AST
- **Echo:** Генератор Markdown-отчётов

## Ссылки

- [Garden-Core Enforcer](../tools/enforcer.md)
- [Агентский Контракт](../spec/agent_contract.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
