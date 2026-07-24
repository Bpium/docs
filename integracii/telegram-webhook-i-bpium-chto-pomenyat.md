---
order: 5
title: Telegram webhook и Бипиум
---

Коротко: если бот шлёт апдейты на внешний запрос в облачный Бипиум, URL нужно брать **через шлюз**, иначе Telegram часто не достучится до облака.

---

## В чём дело

Раньше в webhook telegram писали примерно так:

```
https://ваша-компания.bpium.ru/api/webrequest/имя-запроса
```

С серверов Telegram такой адрес **не открывается** (timeout).

С телефона ссылка при этом может работать -- это нормально, пути разные.

Мы подняли вход снаружи:

```
https://ваша-компания.tg.bpium.ru/api/webrequest/имя-запроса
```

Тот же Бипиум, просто «дверь» для Telegram в другом месте.

---

## Что сделать

1. Откройте в Бипиум каталог **Внешние запросы**, найдите нужный запрос (он должен быть **Активен**).

2. Возьмите ваш `urlId` (хвост адреса после `/api/webrequest/`).

3. Соберите новый URL:

```
https://<ваш-поддомен>.tg.bpium.ru/api/webrequest/<urlId>?async=true
```

Примеры:

-  было: `https://example.bpium.ru/api/webrequest/your_webrequest`

-  стало: `https://example.tg.bpium.ru/api/webrequest/your_webrequest?async=true`

Поддомен тот же, что у кабинета (`xxx` из `xxx.bpium.ru`), только в середине добавили `.tg`.

1. В Telegram Bot API выставьте webhook на этот новый адрес (используйте метод `setWebhook`).

   **POST** (в Postman / curl):

   ```
   POST https://api.telegram.org/bot<token>/setWebhook
   Content-Type: application/json
   
   {
     "url": "https://example.tg.bpium.ru/api/webrequest/your_webrequest?async=true"
   }
   ```

2. Напишите боту любое сообщение и посмотрите в Бипиум каталог **Процессы** -- должен появиться запуск сценария.

По возможности `?async=true` лучше оставить: Telegram быстрее получает ответ «ок», сценарий крутится в фоне.

---

## Кнопки в боте со ссылкой

Если кнопка просто **открывает ссылку** в браузере (не webhook бота) -- можно оставить старый `https://….bpium.ru/api/webrequest/…`.

Шлюз обязателен именно когда **сам Telegram** стучится на URL как на webhook.

---

## Как проверить, что всё ок

После `setWebhook` гляньте `getWebhookInfo`:

-  в `url` есть `tg.bpium.ru`

-  нет ошибки вроде `Connection timed out`

И напишите боту тестовое сообщение -- в Процессах Бипиум должен быть след.

---

## Если не работает

-  Поддомен написали не тот (опечатка в имени компании).

-  Внешний запрос выключен или неверный `urlId`.

-  В webhook всё ещё старый адрес без `.tg.`.

-  Забыли `https://`.

Напишите в поддержку: поддомен, `urlId` и скрин/текст из `getWebhookInfo` (токен бота не присылайте).