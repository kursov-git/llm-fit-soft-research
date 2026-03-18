# Софт Для Бенчмарка Железа И LLM

## Зачем отдельный стек бенчмарка

Для локальных LLM мало знать только:

- объём RAM
- объём VRAM
- размер модели

На практике важны ещё:

- sustained CPU performance
- thermal throttling
- скорость диска
- реальный `tokens/s`
- `time to first token`
- поведение разных quantization-режимов

Поэтому полезно разделять инструменты на 2 слоя:

1. `Hardware baseline`
2. `LLM inference benchmark`

## Recommended stack

Если нужен прагматичный минимум, а не лаборатория, то для edge-хоста вроде `Raspberry Pi 5` я бы держал такой стек:

1. `sysbench`
2. `stress-ng`
3. `fio`
4. `llama.cpp / llama-bench`
5. `LocalScore`
6. `geerlingguy/ai-benchmarks`

## 1. Hardware baseline

### sysbench

- Что это: классический системный бенчмарк для CPU, памяти и file I/O
- Для чего полезен:
  - быстро получить базовый CPU baseline
  - сравнить память и file I/O до и после настройки
  - получить воспроизводимый synthetic baseline
- Когда использовать:
  - в начале, до тестов LLM
  - после смены охлаждения, governor, storage, power supply
- Сильная сторона:
  - очень простой и дешёвый по времени
- Ограничение:
  - не говорит ничего напрямую про качество LLM UX

Источник:

- https://github.com/akopytov/sysbench

### stress-ng

- Что это: stress / soak tool для CPU, памяти, файловой системы и других подсистем
- Для чего полезен:
  - поймать thermal throttling
  - проверить стабильность under sustained load
  - увидеть, как железо ведёт себя не на коротком burst, а на длинной нагрузке
- Когда использовать:
  - перед длинными LLM-тестами
  - после установки активного охлаждения
  - при подозрении, что устройство "на бумаге быстрое, а под нагрузкой проседает"
- Сильная сторона:
  - очень полезен для SBC и мини-компьютеров
- Ограничение:
  - сам автор проекта прямо отмечает, что это не точный benchmark suite, а прежде всего stress tool

Источник:

- https://github.com/ColinIanKing/stress-ng

### fio

- Что это: стандартный low-level benchmark для storage
- Для чего полезен:
  - сравнить `microSD` против `NVMe`
  - понять, насколько storage тормозит загрузку моделей, swap и кэш
- Когда использовать:
  - если выбирается storage-конфигурация
  - если модели на устройстве запускаются заметно медленнее ожиданий
- Сильная сторона:
  - хорошо показывает, где storage реально становится bottleneck
- Ограничение:
  - сам по себе не меряет inference

Источник:

- https://github.com/axboe/fio

### Geekbench 6

- Что это: удобный cross-platform CPU baseline
- Для чего полезен:
  - быстро сравнить `Pi 5` с другими ARM/x86 машинами
  - получить внешне узнаваемый reference score
- Когда использовать:
  - если нужен простой внешний baseline для сравнения хостов
- Сильная сторона:
  - высокая узнаваемость результатов
- Ограничение:
  - не даёт прямого ответа по local LLM inference

Источник:

- https://www.geekbench.com/download/

## 2. LLM inference benchmark

### llama.cpp / llama-bench

- Что это: низкоуровневый benchmark внутри экосистемы `llama.cpp`
- Для чего полезен:
  - измерять throughput на GGUF-моделях
  - сравнивать quants и параметры запуска
  - получать reproducible CPU-only baseline
- Когда использовать:
  - если нужен честный low-level тест именно inference path
  - если хочется сравнить несколько GGUF-квантов одной модели
- Сильная сторона:
  - ближе к реальному inference pipeline, чем системные synthetic tests
- Ограничение:
  - ниже уровень удобства, чем у user-facing benchmark tools

Источник:

- https://github.com/ggml-org/llama.cpp

### LocalScore

- Что это: open benchmark и публичная база результатов для local AI
- Для чего полезен:
  - сравнивать hardware + model combinations
  - опираться на `prompt processing speed`, `generation speed`, `time to first token`
  - получать более user-facing сигнал о качестве локального запуска
- Когда использовать:
  - когда нужны не только локальные замеры, но и внешнее сравнение
  - когда хочется видеть benchmark data в более прикладной форме
- Сильная сторона:
  - соединяет benchmark и data layer
- Ограничение:
  - не заменяет собственные тесты именно на вашем хосте

Источник:

- https://github.com/cjpais/LocalScore

### geerlingguy/ai-benchmarks

- Что это: набор простых AI/LLM benchmarking tools, особенно полезный для edge и homelab-сценариев
- Для чего полезен:
  - быстро прогонять практические AI/LLM тесты
  - использовать готовую основу для сравнений на ARM-хостах
- Когда использовать:
  - если хочется быстрый практический benchmark без изобретения своей harness
  - если интересен edge/homelab angle, включая Raspberry Pi и похожие машины
- Сильная сторона:
  - ближе к реальной прикладной эксплуатации, чем "чистая синтетика"
- Ограничение:
  - это не универсальный industry standard, а pragmatic benchmark toolkit

Источник:

- https://github.com/geerlingguy/ai-benchmarks

## Как я бы мерил Raspberry Pi 5

Для `Raspberry Pi 5 8GB` самый полезный и недорогой по времени сценарий такой:

1. `sysbench`
   Проверить CPU и память в baseline.

2. `stress-ng`
   Проверить, не уходит ли плата в throttling под длинной нагрузкой.

3. `fio`
   Сравнить `microSD` и `NVMe`, если есть выбор.

4. `llama-bench`
   Прогнать 3-4 реальных small models / quants.

5. `LocalScore` или `ai-benchmarks`
   Получить более прикладной сравнительный результат.

## Практический вывод

Если цель не "сделать красивую лабораторию", а реально понять, что потянет edge-хост, то:

- `sysbench + stress-ng + fio` отвечают за железо
- `llama-bench + LocalScore` отвечают за LLM UX

Этого уже достаточно, чтобы без самообмана понять:

- упирается ли система в CPU
- страдает ли она от перегрева
- мешает ли storage
- какие модели реально дают приемлемый интерактивный опыт

## Источники

- https://github.com/akopytov/sysbench
- https://github.com/ColinIanKing/stress-ng
- https://github.com/axboe/fio
- https://www.geekbench.com/download/
- https://github.com/ggml-org/llama.cpp
- https://github.com/cjpais/LocalScore
- https://github.com/geerlingguy/ai-benchmarks
