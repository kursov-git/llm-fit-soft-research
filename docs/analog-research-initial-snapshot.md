# Analog Research: local LLM fit / compatibility tools

Date: 2026-03-18

## Goal

Find the strongest analogous projects to `llmfit` / `canirun.ai` in the space of:

- local LLM hardware-fit checking
- model recommendation for user hardware
- quantization / memory fit planning
- adjacent products that continue the same user journey into actual local execution

## Selection criteria

1. Closeness to the core problem: "what model can run on this machine?"
2. Product maturity and maintenance
3. Practical usefulness to real local-AI users
4. Source quality: repo README, docs, product site

## Top 5

### 1. llmfit

- Type: open-source CLI/TUI
- URL: https://github.com/AlexsJones/llmfit
- Why it matters: currently the cleanest direct match for the problem statement
- Notes:
  - hardware detection for RAM / CPU / GPU
  - scores models by quality, speed, fit, context
  - supports TUI, CLI, plan mode, multi-GPU, MoE, dynamic quant selection
  - integrates with Ollama, llama.cpp, MLX, Docker Model Runner

### 2. CanIRun.ai

- Type: web product
- URL: https://www.canirun.ai/
- Why it matters: best lightweight browser-first version of the same idea
- Notes:
  - tells users which AI models they can run locally
  - explains parameters, quantization, VRAM, MoE, tok/s in plain language
  - browser-based hardware detection
  - estimates are approximate and depend on browser APIs

### 3. Can I Run This LLM? / LocalLLM.run

- Type: web product
- URL: https://www.localllm.run/
- Why it matters: strongest structured web alternative with model DB + browse/compare/guides
- Notes:
  - starts with "find the best local AI model for your hardware"
  - has browse, leaderboard, compare, guides, academy
  - exposes model-level minimum VRAM / RAM and use-case tags
  - closer to a full discovery portal than a simple checker

### 4. LocalScore

- Type: open-source benchmark + public database
- URL: https://github.com/cjpais/LocalScore
- Why it matters: complements estimators with measured performance
- Notes:
  - benchmark tool + public DB for how fast LLMs run on specific hardware
  - metrics: prompt processing speed, generation speed, time to first token
  - useful for grounding recommendations in actual benchmark data
  - currently single-GPU focused

### 5. Jan

- Type: open-source desktop app
- URL: https://github.com/janhq/jan
- Why it matters: not a pure "fit checker", but a strong adjacent product covering the next step after selection
- Notes:
  - open-source offline desktop app for downloading and running local models
  - supports local and cloud models, MCP, OpenAI-compatible local API
  - explicit system requirements in README
  - useful reference for UX after the recommendation step

## Near misses / honorable mentions

### GPT4All

- URL: https://github.com/nomic-ai/gpt4all
- Strong local LLM desktop/app ecosystem and model delivery
- Less focused on hardware-fit recommendation than the top 5

### LocalAI

- URL: https://github.com/mudler/LocalAI
- Strong self-hosted OpenAI-compatible local inference server
- Excellent adjacent backend reference, but not primarily a model-fit advisor

### GGUF Tool Suite

- URL: https://github.com/Thireus/GGUF-Tool-Suite
- Strong niche project for quantization recipes tuned to RAM/VRAM targets
- Valuable for advanced quant planning, but narrower and more expert-facing

## Working ranking logic

If the future product is mainly about recommendation / compatibility:

1. llmfit
2. CanIRun.ai
3. LocalLLM.run
4. LocalScore
5. Jan

If the future product is mainly about end-to-end local execution:

1. Jan
2. GPT4All
3. LocalAI
4. llmfit
5. LocalLLM.run

## Takeaways

- The market splits into 3 layers:
  - fit estimators
  - benchmark databases
  - execution platforms
- `llmfit`, `CanIRun.ai`, and `LocalLLM.run` are the closest direct comps.
- `LocalScore` is the best evidence-driven benchmark layer.
- `Jan`, `GPT4All`, and `LocalAI` are better treated as adjacent execution references than direct substitutes.
