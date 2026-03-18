# Продуктовые гипотезы

## Контекст

Ниже список гипотез не в формате "всё подряд", а в порядке полезности для команды, которая хочет найти рабочую продуктовую позицию в категории `local LLM fit / compatibility intelligence`.

## H1. Recommendation engine важнее, чем просто calculator

### Гипотеза

Пользователю нужен не калькулятор памяти сам по себе, а ранжированный ответ:

- что запускать
- в каком quant
- с каким ожидаемым UX
- почему именно это лучший выбор под его цель

### Почему это важно

Простых VRAM-калькуляторов много. Они слабо защищены от копирования и редко становятся продуктом.

### Что проверить

- готовы ли пользователи предпочесть ranked shortlist вместо raw compatibility table
- какие explanation fields повышают доверие к рекомендации

### MVP

- input: hardware profile + use case
- output: top 3 recommended models с объяснением компромиссов

## H2. Use-case aware ranking даст больше ценности, чем единый общий score

### Гипотеза

Один и тот же хост требует разных рекомендаций для:

- coding assistant
- general chat
- long-context reading
- RAG
- roleplay / creative writing

### Почему это важно

Обычный “best model for your machine” слишком общий и часто вводит в заблуждение.

### Что проверить

- улучшается ли perceived relevance при выборе профиля задачи
- какие use-case сегменты люди выбирают чаще всего

### MVP

- переключатель `use case`
- отдельные веса для speed / context / quality / memory headroom

## H3. Benchmark-backed recommendations повышают доверие и конверсию в запуск

### Гипотеза

Если рекомендация опирается не только на расчёт fit, но и на реальные результаты на похожем железе, пользователи сильнее доверяют продукту и чаще переходят к запуску модели.

### Почему это важно

Пользователь боится ложноположительных рекомендаций:

- "влезет, но будет мучительно медленно"
- "формально работает, но UX непригоден"

### Что проверить

- растёт ли click-through в execution step при показе benchmark evidence
- какие метрики наиболее понятны: tok/s, TTFT, latency, comfort score

### MVP

- confidence badge
- panel с observed performance on similar hardware

## H4. Лучший wedge это handoff в существующие runtime, а не свой runtime

### Гипотеза

На первом этапе выгоднее не строить собственный runtime, а стать intelligence layer поверх:

- Ollama
- llama.cpp
- Jan
- LocalAI

### Почему это важно

Runtime layer уже занят сильными игроками. Быстрее занять позицию recommendation/orchestration layer.

### Что проверить

- насколько для пользователя ценен one-click export в `ollama run`, `Modelfile`, `llama.cpp` args
- какая интеграция нужна первой

### MVP

- export команды запуска
- install / pull handoff
- ready-to-run presets

## H5. Browser-first onboarding хорошо работает для discovery, но host-native агент нужен для точности

### Гипотеза

Двухступенчатая модель даст лучший компромисс:

1. быстрый web discovery
2. optional local probe agent для точной рекомендации

### Почему это важно

Чисто web UX удобен, но ограничен browser APIs. Чисто local UX точнее, но выше порог входа.

### Что проверить

- сколько пользователей готовы установить lightweight probe ради улучшения точности
- насколько растёт quality score рекомендаций после host-native детекта

### MVP

- web app с приблизительным fit
- optional local collector с upload hardware fingerprint

## H6. Отдельный продуктовый слой "context-safe recommendation" недообслужен рынком

### Гипотеза

Пользователи регулярно переоценивают usable context length. Продукт, который честно рекомендует `safe context` под конкретный quant и железо, будет иметь высокую ценность.

### Почему это важно

Почти все говорят "supports X context", но мало кто говорит:

- какой контекст реально комфортен
- на каком железе
- при каком падении скорости

### Что проверить

- насколько часто пользователи меняют решение после показа `safe context` вместо theoretical max context

### MVP

- safe context estimator
- warnings при агрессивных конфигурациях

## H7. Самая сильная B2B версия продукта это advisor для sales / support / infra teams

### Гипотеза

Помимо end-user сегмента, может быть B2B-слой для команд, которые:

- подбирают локальные AI workstation profiles
- стандартизируют developer laptops
- советуют клиентам модели под on-prem deployment

### Почему это важно

B2B use case лучше монетизируется, чем массовый consumer calculator.

### Что проверить

- есть ли спрос на fleet-level recommendation
- нужна ли exportable спецификация: "для такого парка машин рекомендованы такие модели"

### MVP

- batch analysis нескольких hardware profiles
- policy templates: coding workstation, support copilot, private RAG node

## H8. Наиболее защищаемый moat это собственный dataset "hardware x model x quant x outcome"

### Гипотеза

Главная защита продукта не UI и не calculator logic, а накапливаемый dataset:

- hardware fingerprints
- measured runs
- model/quant outcomes
- user feedback on perceived quality

### Почему это важно

UI копируется быстро. Data network effect копируется плохо.

### Что проверить

- можно ли мотивировать пользователей делиться anonymized run telemetry
- какие минимальные поля telemetry дают максимальную ценность

### MVP

- opt-in telemetry
- benchmark/result ingestion pipeline
- confidence score, улучшающийся с ростом данных

## Рекомендуемая последовательность проверки

Если команда хочет двигаться прагматично, то порядок проверки гипотез такой:

1. `H1` recommendation > calculator
2. `H2` use-case aware ranking
3. `H4` integration layer over runtimes
4. `H3` benchmark-backed trust
5. `H5` browser-first + local probe hybrid
6. `H6` context-safe recommendation
7. `H7` B2B advisor layer
8. `H8` proprietary outcome dataset moat

## Самая практичная стартовая позиция

Если резать до одного предложения, то наиболее разумная стартовая ставка выглядит так:

> Не строить ещё один калькулятор VRAM и не строить ещё один runtime, а сделать recommendation intelligence layer для локальных LLM с benchmark-backed ranking и one-click handoff в существующие экосистемы.
