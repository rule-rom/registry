# 🔒 Доступ ограничен: Tier 6 (High Council)

## RTL и топология кремния

Эта страница содержит RTL-код и топологию кремния для Decima8 ASIC.

### Что внутри

- **RTL Source Code** — полный исходный код на Verilog/SystemVerilog
- **Synthesis Scripts** — скрипты для синтеза
- **Place & Route** — данные для размещения и трассировки
- **GDSII** — финальный layout для tape-out
- **Simulation Models** — модели для верификации

### Структура RTL

```
rtl/
├── top/
│   ├── decima8_top.v      # Топ-модуль
│   ├── tile_array.v       # Массив тайлов (16×16)
│   ├── bus16_controller.v # Контроллер BUS16
│   └── vsb_interface.v    # Интерфейс VSB
├── tile/
│   ├── tile_core.v        # Ядро тайла
│   ├── fuse_logic.v       # Логика фьюза
│   ├── decay_unit.v       # Блок затухания
│   ├── weight_matrix.v    # Матрица весов (8×8)
│   └── routing_logic.v    # Логика маршрутизации
├── utils/
│   ├── clamp16.v          # Clamp to 0..15
│   ├── signed_mul.v       # Signed умножение
│   └── accumulator.v      # Аккумулятор
└── testbenches/
    ├── tb_tile.v          # Тест тайла
    ├── tb_array.v         # Тест массива
    └── tb_top.v           # Тест топа
```

### Топология кристалла

```
┌─────────────────────────────────────────────────────────┐
│  Decima8 ASIC Die Layout (25 mm² est.)                  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Tile Array (16×16)                               │  │
│  │  ┌─────┬─────┬─────┬─────┐                        │  │
│  │  │ 0,0 │ 0,1 │ ... │ 0,15│                        │  │
│  │  ├─────┼─────┼─────┼─────┤                        │  │
│  │  │ ... │ ... │ ... │ ... │ 256 tiles total       │  │
│  │  ├─────┼─────┼─────┼─────┤                        │  │
│  │  │15,0 │15,1 │ ... │15,15│                        │  │
│  │  └─────┴─────┴─────┴─────┘                        │  │
│  │                                                   │  │
│  │  BUS16 lanes (8) — горизонтально через весь массив│  │
│  │  VSB lanes (8) — вертикально через весь массив    │  │
│  └───────────────────────────────────────────────────┘  │
│                                                         │
│  Periphery:                                             │
│  - CFG Interface (SPI-like)                             │
│  - EV_FLASH Controller                                  │
│  - READOUT Logic                                        │
│  - Domain Reset Logic                                   │
└─────────────────────────────────────────────────────────┘
```

### Требования к доступу

| Tier | Доступ |
|------|--------|
| Tier 3-4 | ❌ Недоступно |
| Tier 5 (Swarm Node) | ❌ Недоступно |
| Tier 6 (High Council) | ✅ Полный доступ к RTL + топологии |

### Права High Council

**Tier 6** получают:

- ✅ Полный доступ к RTL и топологии
- ✅ Долю в IP ASIC (пропорционально взносу)
- ✅ Прямые права голоса на эволюцию Rule-ROM
- ✅ Эксклюзивный архитектурный аудит от **Root Authority**
- ✅ Право вето на изменения спецификации v0.2+

### Как получить доступ

1. Станьте спонсором уровня **Tier 6 (High Council Elder)** — 1M+ ₽
2. После подтверждения оплаты вы получите:
   - Доступ к закрытому репозиторию RTL
   - Лицензионное соглашение
   - Персональный доступ к **Root Authority**

---

## Ссылки

- [Boosty: Intent-Garden](https://boosty.to/intentgarden)
- [Intent-Garden Support](https://intent-garden.org/support.html)
- [Иерархия Сварма](../spec/hierarchy.md)
- [Фонд ASIC](../support/asic_fund.md)

---

**Bake the Future. Build the Substrate.** 🛠️⚡️
