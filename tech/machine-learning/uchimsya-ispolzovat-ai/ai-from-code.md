# AI from code

## LLM

Работа с LLM на Python: [https://python.langchain.com/docs/get\_started/introduction.html](https://python.langchain.com/docs/get_started/introduction.html)

Единое API для моделек [https://replicate.com/pricing](https://replicate.com/pricing)

### Пример работы с моделями

requirements

```
langchain
langchain-openai
langchain-deepseek
pydantic
pydantic-settings
```

example:

```python
from typing import Optional
import httpx
from functools import partial
from pydantic_settings import BaseSettings
from langchain.chat_models import init_chat_model
from langchain_deepseek import ChatDeepSeek


class _LLMConfig(BaseSettings):
    # llm
    OPENAI_API_KEY: str
    HTTP_PROXY: Optional[str] = None

    class Config:
        env_file = '.env'
        env_file_encoding = 'utf-8'
        extra = 'ignore'


def init_model(base_url: str, model: str):
    config = _LLMConfig()

    return init_chat_model(
        base_url=base_url,
        api_key=config.OPENAI_API_KEY,
        model=model,
        use_responses_api=False,
    )


def _proxy_client(config: _LLMConfig):
    if config.HTTP_PROXY:
        return httpx.Client(
            verify=False,  # Disable SSL verification
            proxy=config.HTTP_PROXY
        )

    return None


def init_deepseek_model(base_url: str, model: str):
    config = _LLMConfig()

    return partial(
        ChatDeepSeek,
        api_base=base_url,
        api_key=config.OPENAI_API_KEY,
        model=model,
        use_responses_api=False,
        http_client=_proxy_client(config),
    )


def main():
    deepseek_v3 = init_deepseek_model(
        base_url='https://deepseek/...',
        model='deepseek-v3-model-id-...'
    )
    prompt = 'Какую версию модели ты используешь?'
    model = deepseek_v3(temperature=0, max_tokens=1500)
    response = model.invoke(prompt)
    print('Ответ модели', response)


if __name__ == '__main__':
    main()

```

## ML

### Lead projects

PyTorch (by Facebook)

TenserFlow (by Google)

Keras

`Foolbox` - wrapper над другими пакетами по нейронкам [https://github.com/bethgelab/foolbox](https://github.com/bethgelab/foolbox)

[Advertorch](https://github.com/BorealisAI/advertorch)\
[CleverHans](https://github.com/cleverhans-lab/cleverhans)

### Python

matplotlib \
numpy \
pandas \
scipy \
scikit-learn ([книжка](https://disk.yandex.ru/i/ObNXcwiXvspYTw))
