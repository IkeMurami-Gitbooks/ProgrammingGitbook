# Telegram Bot API

[https://github.com/LibreLabUCM/teleg-api-bot/wiki/Getting-started-with-the-Telegram-Bot-API](https://github.com/LibreLabUCM/teleg-api-bot/wiki/Getting-started-with-the-Telegram-Bot-API)

Регистрируем своего бота у бота [@BotFath](https://t.me/botfather). Получаем auth token (если этот токен ликнут, то получат доступ к нашему боту).

Samples: [https://core.telegram.org/bots/samples](https://core.telegram.org/bots/samples)

Пример на aiogram:

```python
import asyncio
from aiogram import Bot

async def main():
    bot = Bot(token='YOUR-TOKEN')
    resp = await bot.get_me()
    print(resp)

if __name__ == '__main__':
    asyncio.run(main())
```
