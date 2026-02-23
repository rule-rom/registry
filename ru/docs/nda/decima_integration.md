# 🔒 Decima8: Интерфейс Decima-API

**Версия:** v0.2  
**Статус:** DESIGN FREEZE

---

## 🔐 Доступ ограничен: Tier 3+ (Промышленник)

Эта страница содержит документацию по API для взаимодействия с Decima8.

> **Внимание:** Данный документ содержит детали SHM ABI, структуры данных и примеры кода. Доступ только для участников **Tier 3+**.

---

## 1) Event Protocol (API)

### Внешние события

| Событие | Описание | Требования |
|---------|----------|------------|
| **EV_FLASH(tag_u32)** | Выполняет один детерминированный цикл READ→WRITE | BAKE_APPLIED==1 |
| **EV_RESET_DOMAIN(mask16)** | Сброс доменов | Только между EV_FLASH |
| **EV_BAKE()** | Применение BakeBlob атомарно | Только между EV_FLASH |

### Внутренние подфазы EV_FLASH

```
1. PHASE_READ         — все тайлы семплируют вход
2. TURNAROUND         — Conductor: Hi-Z VSB, Island: prepare drive
3. PHASE_WRITE        — Island драйвит BUS16
4. READOUT_SAMPLE     — Conductor читает BUS16[0..7]
5. INTERPHASE_AUTORESET — опционально
```

---

## 2) SHM ABI (Shared Memory)

### Структура разделяемой памяти

```
┌─────────────────────────────────────────────────────────┐
│  SHM Region (~64 KB)                                    │
│  ┌───────────────────────────────────────────────────┐  │
│  │ Header (статус, bake_id, profile_id, flags)       │  │
│  ├───────────────────────────────────────────────────┤  │
│  │ Config Staging (BakeBlob buffer)                  │  │
│  ├───────────────────────────────────────────────────┤  │
│  │ VSB_INGRESS (входной вектор)                      │  │
│  ├───────────────────────────────────────────────────┤  │
│  │ OUT_buf (readout + FLAGS32)                       │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### Характеристики

| Параметр | Значение |
|----------|----------|
| **Выравнивание** | 4096 bytes (page-aligned) |
| **Протокол** | Lock-free ring buffer |
| **Задержка** | <1 µs на операцию |

---

## 3) API Функции

### Инициализация

```
decima_init(shm_path)      — инициализация SHM
decima_shutdown()          — закрытие
```

### Конфигурация

```
decima_bake_prepare(blob)  — загрузка BakeBlob в staging
decima_bake_apply()        — применение (EV_BAKE)
decima_reset_domains(mask) — сброс доменов
```

### Выполнение

```
decima_set_vsb_ingress(input) — установка входа
decima_ev_flash(tag)          — выполнение цикла
decima_get_readout()          — чтение результата
decima_get_flags32()          — чтение FLAGS
```

### Статус

```
decima_is_ready()            — проверка готовности
decima_get_bake_id()         — текущий bake_id
decima_get_profile_id()      — текущий profile_id
```

---

## 4) Ошибки EV_BAKE

| Код | Название | Описание |
|-----|----------|----------|
| 0 | OK | Успех |
| -1 | BakeBadMagic | Неверный magic |
| -2 | BakeBadVersion | Неверная версия |
| -3 | BakeBadLen | Неверная длина |
| -4 | BakeMissingTLV | Отсутствует TLV |
| -5 | BakeBadTLVLen | Неверная длина TLV |
| -6 | BakeCRCFail | Ошибка CRC32 |
| -7 | BakeReservedNonZero | Reserved ≠ 0 |
| -8 | TopologyMismatch | Несоответствие топологии |
| -9 | BakeNoBlob | Нет BakeBlob |

---

## 5) Readout Timing

### Default R0_RAW_BUS

Conductor читает BUS16[0..7] сразу после завершения PHASE_WRITE.

### Задержки (target)

| Операция | Задержка |
|----------|----------|
| EV_FLASH | ~40 µs (i5-3550) |
| SHM read | <1 µs |
| EV_BAKE | ~100 µs |

---

## 6) CFG Interface (SPI-like)

Через CFG интерфейс (CFG_CS, CFG_SCLK, CFG_MOSI, CFG_MISO):

- Загрузка BakeBlob в staging
- Чтение FLAGS
- Команда EV_RESET_DOMAIN(mask16)
- Чтение BAKE_ID_ACTIVE / PROFILE_ID_ACTIVE (опционально)

---

## Как получить полный доступ

1. Станьте спонсором уровня **Tier 3 (Industrialist)** или выше
2. После подтверждения вы получите:
   - Полное API с примерами кода
   - Reference implementation
   - Документацию по интеграции

---

## Ссылки

- [Спецификация машины](decima_contract.md)
- [Форматы TLV и UDP](formats.md)
- [Бинарники IDE](ide_binaries.md)
- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Иерархия Сварма](../spec/hierarchy.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
