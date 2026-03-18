# Raspberry Pi 5 8GB: Топ-10 Локальных Моделей

## Короткий вывод

`Raspberry Pi 5 8GB` это машина для `small language models`, а не для комфортного класса `7B-14B`.

Практически это означает:

- комфортная зона: `0.5B-2B`
- рабочая, но уже компромиссная зона: `3B-4B`
- `7B/8B` и выше на CPU-only Pi 5 обычно либо слишком медленные, либо вообще не стоят усилий

По открытым тестам на `Raspberry Pi 5 8GB`:

- `qwen2.5-0.5b-q4`: около `19.41 tok/s`
- `phi3.5-3.8b-q4`: около `3.42 tok/s`
- `phi3-3.8b-q4`: около `3.06 tok/s`
- `gemma2-2b-q4`: около `2.97 tok/s`
- `mistral-7b-q4`: около `0.97 tok/s`

Это хороший ориентир, почему на таком железе стоит мыслить малыми моделями.

## Важные оговорки

- Ниже это `top 10 to try`, а не "абсолютно лучшие модели рынка".
- Ранжирование сделано под `Pi 5 8GB` без внешнего AI-ускорителя.
- При `microSD`, плохом охлаждении и фоне из других сервисов результат будет хуже.
- Если будет `NVMe + active cooling`, опыт будет заметно стабильнее.

## Топ-10

| Rank | Model | Approx size | Primary use | Fit on Pi 5 8GB | Why it is in the list |
| --- | --- | --- | --- | --- | --- |
| 1 | `qwen2.5:1.5b` | 986MB | general chat, multilingual | Excellent | лучший баланс скорости, качества и размера |
| 2 | `granite3.3:2b` | 1.5GB | general tasks, RAG, structured prompts | Excellent | сильный универсальный 2B-класс, длинный контекст |
| 3 | `gemma3:1b` | 815MB | chat, edge assistant | Excellent | очень маленький и практичный базовый general model |
| 4 | `qwen2.5-coder:1.5b` | 986MB | code, scripts, small fixes | Excellent | лучший coding-first старт на этом железе |
| 5 | `llama3.2:1b` | 1.3GB | summarization, retrieval, rewrite | Very good | хороший edge-friendly multilingual model |
| 6 | `qwen2.5:0.5b` | 398MB | ultra-fast assistant, command helper | Excellent | если нужен максимум скорости и минимум памяти |
| 7 | `smollm2:1.7b` | 1.8GB | lightweight general tasks | Very good | хороший компактный запасной general model |
| 8 | `qwen2.5:3b` | 1.9GB | best-quality general model still sensible on Pi | Good / borderline | верхняя разумная граница для общего назначения |
| 9 | `qwen2.5-coder:3b` | 1.9GB | best-quality coding model still sensible on Pi | Good / borderline | верхняя разумная граница для coding use case |
| 10 | `phi3.5:3.8b` | 2.2GB | reasoning-ish tasks, experiments | Borderline | полезен как upper-bound experiment, но уже тяжёлый |

## Почему именно эти модели

### 1. qwen2.5:1.5b

- Почему высоко:
  - компактный
  - multilingual
  - для edge-устройства даёт очень хороший practical balance
- Когда брать первым:
  - если нужен один "основной" general-purpose кандидат

### 2. granite3.3:2b

- Почему высоко:
  - хорош для `RAG`, question-answering, summarization и structured tasks
  - заявлен с `128K` context, хотя на Pi к большому контексту надо относиться осторожно
- Когда брать:
  - если важны document tasks и structured prompting

### 3. gemma3:1b

- Почему высоко:
  - очень маленький
  - подходит как быстрый локальный ассистент
- Когда брать:
  - если нужен лёгкий everyday local model

### 4. qwen2.5-coder:1.5b

- Почему высоко:
  - coding-specific семейство
  - практичнее, чем пытаться тянуть на Pi большие coder-модели
- Когда брать:
  - shell tasks, code snippets, объяснение кода, мелкие фиксы

### 5. llama3.2:1b

- Почему в списке:
  - очень адекватный edge-sized candidate
  - хороший компромисс между размером и общими chat-задачами
- Когда брать:
  - summarization, rewrite, assistant tasks

### 6. qwen2.5:0.5b

- Почему в списке:
  - самый быстрый practical fallback
  - полезен, когда latency важнее качества
- Когда брать:
  - offline helper, CLI-помощник, простые диалоги

### 7. smollm2:1.7b

- Почему в списке:
  - специально позиционируется как on-device friendly compact family
  - хороший дополнительный кандидат для сравнения с Qwen/Granite/Gemma
- Когда брать:
  - если хочется очень лёгкую general модель для простых задач

### 8. qwen2.5:3b

- Почему в списке:
  - это уже "верхняя" general-purpose зона, где Pi 5 ещё имеет смысл
- Когда брать:
  - если хочется выжать максимум качества без перехода к явно мучительным `7B`

### 9. qwen2.5-coder:3b

- Почему в списке:
  - лучший способ попробовать более серьёзный coding-first вариант без прыжка в `7B`
- Когда брать:
  - если `1.5b` coder слишком прост, а `7b` на Pi уже неадекватен

### 10. phi3.5:3.8b

- Почему в списке:
  - открытые тесты на Pi 5 показывают, что модель ещё работает на уровне около `3.42 tok/s`
- Когда брать:
  - как контрольную "тяжёлую" точку для сравнения
- Почему низко:
  - для повседневного интерактивного UX уже тяжеловата

## Что я не рекомендую как основной план

### 7B / 8B class

На `Pi 5 8GB` этот класс обычно выглядит так:

- запускается медленно
- отвечает медленно
- даёт мало practical payoff относительно `3B`

Открытый пример:

- `mistral-7b-q4` на `Pi 5 8GB` показал около `0.97 tok/s`

То есть это скорее "технически можно", чем "реально удобно".

### Очень старые micro-models как main choice

Модели типа `TinyLlama` всё ещё полезны как ультралёгкий fallback, но сегодня разумнее сначала пробовать более свежие small families вроде:

- `Qwen2.5`
- `Qwen2.5-Coder`
- `Gemma 3`
- `Granite 3.3`
- `SmolLM2`

## Рекомендуемый стартовый набор

Если хочется не утонуть в выборе, я бы начал с таких 4 моделей:

1. `qwen2.5:1.5b`
2. `granite3.3:2b`
3. `qwen2.5-coder:1.5b`
4. `qwen2.5:3b`

Этого уже достаточно, чтобы понять:

- нужен ли тебе упор в скорость
- нужен ли тебе упор в coding
- стоит ли вообще подниматься выше `2B-3B`

## Если потом захочется усилить Pi 5

Самые практичные апгрейды:

1. `NVMe`
   Лучше, чем полагаться на `microSD`.

2. `Active cooling`
   Важен для стабильной длинной нагрузки.

3. `Raspberry Pi AI HAT+ 2`
   Отдельно интересен, потому что у него свои `8GB` памяти на плате и Raspberry Pi прямо пишет, что он позволяет запускать `LLM/VLM up to ~6B parameters`.

Важно:

- обычный `AI HAT+` для LLM не подходит
- `AI HAT+ 2` это уже отдельный сценарий, не тот же самый baseline, что у голого `Pi 5 8GB`

## Практический вывод

Если цель не "доказать, что 7B формально заводится", а реально пользоваться устройством, то `Pi 5 8GB` стоит рассматривать как платформу для:

- `0.5B-2B` в ежедневной работе
- `3B-4B` в режиме эксперимента и компромисса

На сегодня лучший practical sweet spot для такого хоста выглядит как:

- `Qwen2.5 1.5B`
- `Granite 3.3 2B`
- `Qwen2.5-Coder 1.5B`

## Источники

- Raspberry Pi 5 product brief: https://datasheets.raspberrypi.com/rpi5/raspberry-pi-5-product-brief.pdf
- Ollama Linux ARM64: https://docs.ollama.com/linux
- Ollama library `qwen2.5`: https://ollama.com/library/qwen2.5
- Ollama library `qwen2.5-coder`: https://ollama.com/library/qwen2.5-coder
- Ollama library `gemma3`: https://ollama.com/library/gemma3
- Ollama library `granite3.3`: https://ollama.com/library/granite3.3
- Ollama library `llama3.2`: https://ollama.com/library/llama3.2
- Ollama library `smollm2`: https://ollama.com/library/smollm2
- Ollama library `phi3.5`: https://ollama.com/library/phi3.5
- DFRobot benchmark on Raspberry Pi 5 8GB: https://www.dfrobot.com/blog-14068.html
- Raspberry Pi AI HAT+ docs: https://www.raspberrypi.com/documentation/accessories/ai-hat-plus.html
