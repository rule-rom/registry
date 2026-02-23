# 🔒 Доступ ограничен: Tier 4+ (Swarm Founder)

## Swarm-Bus API

Эта страница содержит документацию по API для работы с шиной Swarm-Bus.

### Что внутри

- **Swarm-Bus API Specification** — полное описание протокола
- **UDP Protocol (packet_v1)** — спецификация пакетов для каскадирования машин
- **SHM ABI** — ABI для работы через разделяемую память
- **Event Protocol** — протокол событий (EV_FLASH, EV_RESET_DOMAIN, EV_BAKE)

### Формат пакета UDP (37 bytes)

```
Offset  Size  Field              Description
──────  ────  ─────────────────  ───────────────────────────
0       4     magic              'D8UP' (0x50553844)
4       2     version            = 1
6       2     flags              bit0:has_winner, bit1:has_bus, ...
8       4     frame_tag          Уникальный ID кадра
12      1     domain_id          ID домена (0..15)
13      2     pattern_id         ID паттерна
15      2     reset_mask16       Маска сброса доменов
17      2     collision_mask16   Маска коллизий
19      2     winner_tile_id     ID победителя (при коллизии)
21      4     cycle_time_us      Время цикла в мкс
25      4     flags32_last       FLAGS32 из последнего EV_FLASH
29      8     bus16[8]           Значения BUS16[0..7]
──────  ────  ─────────────────  ───────────────────────────
Total: 37 bytes
```

### Event Protocol (API)

| Событие | Описание | Требования |
|---------|----------|------------|
| `EV_FLASH(tag_u32)` | Выполняет цикл READ→WRITE | BAKE_APPLIED==1 |
| `EV_RESET_DOMAIN(mask16)` | Сброс доменов | Между EV_FLASH |
| `EV_BAKE()` | Применение BakeBlob | Между EV_FLASH |

### Требования к доступу

| Tier | Доступ |
|------|--------|
| Tier 3 (Industrialist) | ❌ Недоступно |
| Tier 4 (Swarm Founder) | ✅ API документация + примеры |
| Tier 5 (Swarm Node) | ✅ + исходники reference implementation |
| Tier 6 (High Council) | ✅ полный доступ + права на расширение |

### Пример использования (C++)

```cpp
// Инициализация Swarm-Bus
SwarmBus bus;
bus.init("/dev/shm/decima8");

// Подготовка входного вектора
Level16 input[8] = {5, 10, 3, 8, 12, 0, 7, 15};
bus.set_vsb_ingress(input);

// Выполнение EV_FLASH
auto readout = bus.ev_flash(0x12345678);

// Чтение результата
for (int i = 0; i < 8; i++) {
    printf("BUS16[%d] = %d\n", i, readout[i]);
}
```

### Как получить доступ

1. Станьте спонсором уровня **Tier 4 (Swarm Founder)** или выше
2. После подтверждения оплаты вы получите доступ к API документации

---

## Ссылки

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Intent-Garden Support](https://intent-garden.org/support.html)
- [Иерархия Сварма](../spec/hierarchy.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
