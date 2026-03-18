# Сравнительная матрица

## Легенда

- `High` = сильная сторона продукта
- `Medium` = частично закрыто
- `Low` = слабое покрытие

## Top 5

| Product | Type | Hardware detection | Model recommendation | Benchmark evidence | Execution handoff | UX accessibility | Maturity | Best for |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `llmfit` | CLI/TUI | High | High | Medium | High | Medium | High | power users, host-native fit checks |
| `CanIRun.ai` | Web app | Medium | High | Low | Low | High | Medium | instant browser-first compatibility checks |
| `LocalLLM.run` | Web portal | Medium | High | Medium | Medium | High | Medium | discovery, compare, guided exploration |
| `LocalScore` | Benchmark/data | Low | Medium | High | Low | Medium | Medium | evidence-based performance validation |
| `Jan` | Desktop app | Medium | Medium | Low | High | High | High | post-selection local usage |

## Adjacent products

| Product | Type | Hardware detection | Model recommendation | Benchmark evidence | Execution handoff | UX accessibility | Maturity | Why not top 5 direct analog |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `GPT4All` | Desktop app | Medium | Medium | Low | High | High | High | stronger as execution product than as fit advisor |
| `LocalAI` | Self-hosted server | Low | Low | Low | High | Low | High | backend runtime, not user-facing selection engine |
| `Ollama` | Runtime/distribution | Low | Low | Low | High | High | Very High | essential runtime layer, but not a recommendation product |
| `GGUF Tool Suite` | Specialist tooling | Medium | Medium | Low | Medium | Low | Medium | valuable for advanced quant workflows, too niche for main comp set |

## Разбор по продуктовым осям

### 1. Кто лучше решает core-job "что мне запускать?"

1. `llmfit`
2. `CanIRun.ai`
3. `LocalLLM.run`
4. `LocalScore`
5. `Jan`

### 2. Кто лучше решает "я уже выбрал и хочу сразу работать?"

1. `Jan`
2. `GPT4All`
3. `Ollama`
4. `LocalAI`
5. `llmfit`

### 3. Кто лучше даёт доверие к рекомендациям?

1. `LocalScore`
2. `llmfit`
3. `LocalLLM.run`
4. `CanIRun.ai`
5. `Jan`

## Основные рыночные гэпы

### Gap 1: fit без доказательности

Большинство fit-checkers умеют сказать:

- поместится / не поместится
- примерный класс модели

Но не умеют уверенно сказать:

- какой будет реальный tok/s
- какой будет `time to first token`
- насколько комфортным будет UX в coding/chat/RAG сценариях

### Gap 2: benchmark без нормального recommendation layer

Benchmark-решения хорошо собирают фактические данные, но редко превращают их в:

- понятный ranking
- user-specific recommendations
- готовую следующую action step

### Gap 3: execution без умного выбора

Execution-платформы отлично запускают модели, но обычно слабо помогают ответить:

- какая модель лучше именно для этого железа
- какой quant на самом деле оптимален
- какой компромисс между качеством и latency стоит выбрать

## Что выглядело бы как сильный продукт

Продукт уровня category winner в этой нише должен объединить:

1. точный local hardware probe
2. актуальный model/quant knowledge graph
3. benchmark-backed ranking
4. use-case aware recommendation
5. one-click запуск через популярные runtime-платформы
