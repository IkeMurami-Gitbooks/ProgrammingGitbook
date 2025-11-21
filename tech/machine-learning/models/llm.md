# llm

## Public (or OSS)

LLaMa — языковая модель от Facebook: [https://ai.facebook.com/blog/large-language-model-llama-meta-ai/](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)

LMSYS Projects: [https://lmsys.org/projects/](https://lmsys.org/projects/)

У LMSYS есть проект над LLM от Facebook (LLaMa) над векторами от Vicuna: [https://github.com/lm-sys/FastChat#fine-tuning-vicuna-7b-with-local-gpus](https://github.com/lm-sys/FastChat#fine-tuning-vicuna-7b-with-local-gpus)

Можно использовать в своих целях: пару месяцев обучения на корпусе (есть есть вычислительные мощности), проверка экспертов, дообучение. Если для себя, то можно взять уже готовые модели, например, Vicuna model

MoonshotAI, модель [Kimi](https://www.kimi.com/en/)

## Proprietary

* OpenAI
  * Модели: [https://platform.openai.com/docs/models](https://platform.openai.com/docs/models)
    * gpt-4.1, gpt-5, gpt-5.1 и gpt-5-pro: text and image to text, самые дорогие
      * 4.1 — можно дообучать на примерах (fine-tuning)
      * 5-pro — супер рассуждающая
    * gpt-5-nano — самая дешевая и быстрая версия gpt-5. Подходит для тасков суммаризации и классификации
    * gpt-5-mini — подороже gpt-5-nano, хороша для хорошо сформулированных задач и точных промптов
    * gpt-oss-20b и gpt-oss-120b — две модели text-to-text с открытыми весами&#x20;
* Anthropic — [Claude](https://docs.claude.com/en/docs/about-claude/models/overview)
  * Claude 3 Opus: мощная и дорогая модель, для сложных задач
  * Claude 3 Sonnet: сбалансированная модель, для масштабируемых сценариев
  * Claude 3 Haiku: быстрая и компактная модель
  * Есть свой пакет — `pip install anthropic`
* Cohere: поддерживает порядка 10 языков, а не только английский
  * Модели Command R / Command R+
* Google — [Gemini](https://ai.google.dev/gemini-api/docs/models) (ранее Bard) на основе языковой модели LaMDA
  * Gemini 2.5 Pro: большое контекстное окно, что делает модель подходящей для задач обработки большого массива данных
  * Gemini 2.5 Flash: облегченная версия Gemini 2.5 Pro
* xAI — [Grok 4](https://docs.x.ai/docs/models)
* [DeepSeek](https://api-docs.deepseek.com/quick_start/pricing) (на [huggingface](https://huggingface.co/deepseek-ai))
* Alibaba — [Qwen и QwQ](https://www.alibabacloud.com/help/en/model-studio/models)
  * Здесь можно попробовать Qwen и QwQ: [https://huggingface.co/Qwen](https://huggingface.co/Qwen)
  * Qwen — основная модель общего назначения
  * QwQ — reasoning-модель
* Perplexity: [https://docs.perplexity.ai/getting-started/pricing#sonar-models-chat-completions](https://docs.perplexity.ai/getting-started/pricing#sonar-models-chat-completions)
  * Модель Sonar и другие

## Others

ByteDance:

* [https://huggingface.co/ByteDance/models](https://huggingface.co/ByteDance/models)
* [https://huggingface.co/bytedance-research/models](https://huggingface.co/bytedance-research/models)

Tencent:

* [https://huggingface.co/tencent/models](https://huggingface.co/tencent/models)
* [https://huggingface.co/TencentARC/models](https://huggingface.co/TencentARC/models)
* [https://huggingface.co/Tencent-Hunyuan/models](https://huggingface.co/Tencent-Hunyuan/models)

Kolors: [https://github.com/Kwai-Kolors](https://github.com/Kwai-Kolors) — исследовательская команда Kuaishou/Kwai

Alibaba PAI: [https://huggingface.co/alibaba-pai/models](https://huggingface.co/alibaba-pai/models)

Microsoft: [https://huggingface.co/microsoft/models](https://huggingface.co/microsoft/models)

Apple: [https://huggingface.co/apple/models](https://huggingface.co/apple/models)

Beijing Academy of AI: [https://huggingface.co/BAAI/models](https://huggingface.co/BAAI/models)

Shenzhen Institute of Data Economy (IDEA Research): [https://huggingface.co/IDEA-CCNL/models](https://huggingface.co/IDEA-CCNL/models)

* [https://github.com/IDEA-CCNL/Fengshenbang-LM](https://github.com/IDEA-CCNL/Fengshenbang-LM)

ETRI Vision Intelligence Lab: [https://huggingface.co/etri-vilab/models](https://huggingface.co/etri-vilab/models)

