# MVP Definition

## Цель MVP

Не построить runtime и не построить большой data platform, а проверить, нужна ли пользователю `recommendation intelligence layer` для локальных LLM.

## Рабочее определение MVP

MVP отвечает на один конкретный вопрос:

> Какие 3 модели и какие кванты мне реально стоит попробовать на моём железе под мой use case, и почему именно их?

## Целевой пользователь первого прохода

Наиболее практичный первый сегмент:

- технический пользователь
- уже знает про Ollama / llama.cpp / Jan / GPT4All
- хочет запускать модели локально
- не хочет вручную разбираться в параметрах, квантовании и контексте

Не первый сегмент:

- массовый consumer без интереса к локальным моделям
- enterprise buyer с procurement cycle
- команды, которым нужен полноценный fleet management продукт

## Core user job

`Help me choose the best local LLM setup for my machine and my task without wasting hours on trial and error.`

## Что MVP принимает на вход

Минимальный набор входов:

1. hardware profile
   - RAM
   - GPU vendor
   - VRAM
   - CPU class

2. use case
   - coding
   - chat
   - RAG / reading
   - long context

3. preference mode
   - best quality
   - balanced
   - best speed

## Что MVP должен возвращать

На выходе обязательно должны быть:

1. `Top 3 recommendations`
   - model
   - quant
   - expected fit class

2. `Why this is recommended`
   - краткое объяснение компромисса
   - почему это лучше альтернатив для выбранного режима

3. `Expected user experience`
   - приблизительный comfort label
   - safe context guidance
   - рискованные места конфигурации

4. `Next action`
   - команда для запуска через Ollama или другой runtime
   - ближайшая альтернативная модель, если пользователь хочет больше quality или больше speed

## Что MVP не должен делать

- не должен быть полноценным runtime
- не должен требовать собственной model hosting platform
- не должен покрывать все модели рынка
- не должен обещать высокоточную производительность без evidence layer
- не должен превращаться в general-purpose AI app

## Пример выхода MVP

### Input

- 32 GB RAM
- 12 GB VRAM
- use case: coding
- mode: balanced

### Output

1. `Qwen2.5-Coder 14B Q4`
   Подходит лучше всего как баланс качества и укладываемости в VRAM.

2. `DeepSeek Coder class ~7B Q4/Q5`
   Чуть проще по качеству, но даёт лучший запас по скорости и стабильности.

3. `Smaller coding model ~7B high-quant`
   Хороший safe fallback, если важен комфортный интерактивный UX.

И рядом:

- safe context recommendation
- краткое пояснение, почему 32B class здесь уже рискован
- ready-to-run command

## Какой минимальный trust layer нужен

Даже самый ранний MVP должен иметь хотя бы один из этих trust mechanisms:

1. transparent reasoning
   Пользователь видит, почему рекомендация сделана.

2. confidence label
   Например: `high confidence`, `medium confidence`, `experimental`.

3. evidence hints
   Даже если benchmark layer ещё слабый, нужно показать, где оценка расчётная, а где подтверждена практикой.

Без trust layer MVP будет восприниматься как ещё один случайный калькулятор.

## Что является success signal для MVP

Положительные сигналы:

1. пользователь считает shortlist полезнее, чем raw model table
2. пользователь понимает, что запускать дальше, без дополнительного ресёрча
3. пользователь доверяет объяснению рекомендации
4. handoff в runtime выглядит естественным продолжением

Негативные сигналы:

1. пользователь всё равно идёт сравнивать десятки моделей вручную
2. рекомендации кажутся слишком общими
3. нет доверия без benchmark evidence
4. handoff не нужен, потому что люди и так уже знают, что запускать

## Самая практичная форма MVP

Если мыслить максимально прикладно, то MVP скорее всего должен быть:

- либо lightweight web flow
- либо локальный CLI/helper
- либо hybrid: web discovery + local probe

На исследовательской стадии наиболее рационален такой вариант:

1. простой structured recommendation flow
2. ограниченный набор моделей
3. объяснимый ranking
4. handoff в Ollama

## Почему именно такой MVP

Потому что он проверяет самую важную гипотезу:

`нужен ли пользователю интеллектуальный выбор и объяснение, а не просто калькулятор совместимости`

При этом он:

- не требует строить runtime
- не требует собственной inference-инфраструктуры
- не требует сразу собирать огромный benchmark dataset
- остаётся достаточно конкретным, чтобы давать реальную практическую пользу

## Практическое резюме

Если упростить до одной строки:

> MVP этой категории должен рекомендовать не "что теоретически помещается", а "что реально стоит попробовать первым, в каком виде и почему".
