# Валидация типов (TYPE-INTENT)

**Intent ID:** `:type-safe-cast`  
**Статус:** 🚧 Draft  
**Версия:** 0.1

## Описание

Контракт для безопасного приведения типов в C/C++. Обеспечивает трассировку приведений и валидацию совместимости типов.

## EDN Спецификация (Draft)

```edn
{:intent :type-safe-cast
 :entities {:source {:type "T*" :role :cast-from}
            :target {:type "U*" :role :cast-to}}
 :invariants [
   ;; Правило: приведение должно быть явным
   {:op :cast :from :source :to :target
    :require-explicit true}
   
   ;; Правило: запрет опасных приведений
   {:forbid {:cast :from :const-ptr :to :non-const-ptr}}
 ]
 :tags {:wrap "// [[garden:type-checked]]"}}
```

## Пример использования

### ✅ Валидный код

```c
// [[garden:intent(type-safe-cast)]]
void process_data(const void* data) {
    // Явное приведение с сохранением const
    const int* values = (const int*)data;
    process_values(values);
}
// [[/garden:intent]]
```

### ❌ Нарушение

```c
// [[garden:intent(type-safe-cast)]]
void unsafe_cast(const void* data) {
    // ОШИБКА: снятие const без явного намерения
    int* values = (int*)data;  // Нарушение безопасности типов
}
// [[/garden:intent]]
```

## Статус разработки

Этот интент находится в разработке. Для участия в тестировании обратитесь к [High Council](../spec/hierarchy.md).

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
