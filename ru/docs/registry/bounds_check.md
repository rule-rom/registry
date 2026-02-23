# Контроль границ (BOUNDS)

**Intent ID:** `:bounds-checked-access`  
**Статус:** 🚧 Draft  
**Версия:** 0.1

## Описание

Контракт для проверки границ массивов и буферов. Обеспечивает детерминированную валидацию индексов перед доступом к памяти.

## EDN Спецификация (Draft)

```edn
{:intent :bounds-checked-access
 :entities {:array {:type "T[]" :role :bounded-buffer}
            :index {:type "size_t" :role :access-key}}
 :invariants [
   ;; Правило: индекс должен быть в пределах [0, size)
   {:op :access :target :array :index :index
    :precondition {:and 
      [{:gte :index 0}
       {:lt :index :size}]}}
 ]
 :tags {:wrap "// [[garden:bounds-checked]]"}}
```

## Пример использования

### ✅ Валидный код

```c
// [[garden:intent(bounds-checked-access)]]
int safe_array_access(int* arr, size_t size, size_t index) {
    if (index < size) {  // Проверка границы
        return arr[index];
    }
    return -1;  // Ошибка выхода за границы
}
// [[/garden:intent]]
```

### ❌ Нарушение

```c
// [[garden:intent(bounds-checked-access)]]
int unsafe_access(int* arr, size_t size, size_t index) {
    return arr[index];  // ОШИБКА: нет проверки границы
}
// [[/garden:intent]]
```

## Статус разработки

Этот интент находится в разработке. Для участия в тестировании обратитесь к [High Council](../spec/hierarchy.md).

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
