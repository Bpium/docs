---
description: >-
  Сообщения - ресурс платформы, манипулирующий с сообщениями в каждой записи в
  Бипиуме.
title: Сообщения (Messages)
order: 1.9
---

Сообщения - ресурс платформы, манипулирующий с сообщениями в каждой записи в Бипиуме.

## Получить сообщение с записи

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/records/{recordId}/messages
```

Метод: **GET**

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `recordId` (number) -- идентификатор записи

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
[
  {
    "id": "52",
    "catalogId": "$messages",
    "author": {
      "sectionId": "1",
      "sectionDbId": 1,
      "catalogDbId": 3,
      "catalogId": "$users",
      "catalogTitle": "Сотрудники",
      "catalogIcon": "users-1",
      "recordDbId": 11,
      "recordId": "11",
      "recordTitle": "admin",
      "isRemoved": false
    },
    "attachments": [],
    "createdDate": "2024-06-04T04:24:44.146Z",
    "deleted": false,
    "deletedDate": null,
    "mention": [],
    "reply": null,
    "text": "Привет!",
    "updatedDate": null
  },
]
```

[/tab]

[/tabs]

## Создать сообщение

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/records/{recordId}/messages
```

Метод: **POST**

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `recordId` (number) - идентификатор записи

Запрос: (application/json)

```javascript
{
  "text": "Как дела?",
  "mentions": [],
  "attachments": [],
  "replyMessageId": null
}
```

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
{
    "id": "7" // идентификатор созданного вида
}
```

[/tab]

[/tabs]

## Изменить сообщение

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/records/{recordId}/messages/{messageId}
```

Метод: **PATCH**

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `recordId` (number) - идентификатор записи

-  `messageId` (number) - идентификатор сообщения

Запрос: (application/json)

```javascript
{
  "text": "hello world",
  "mentions": [],
  "attachments": [],
  "replyMessageId": null
}
```

[/tab]

[tab:Ответ]

Ответ: 200 ОК

[/tab]

[/tabs]

## Удалить сообщение

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/records/{recordId}/messages/{messageId}
```

Метод: **DELETE**

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `recordId` (number) - идентификатор записи

-  `messageId` (number) - идентификатор сообщения

[/tab]

[/tabs]

## Подписаться на сообщения в записи

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/records/{recordId}/chatOptions/{recordId}
```

Метод: `PATCH`

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `recordId` (number) - идентификатор записи

Запрос: (application/json)

```
{
  "subscribe": true
}
```

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```
{subscribe: true}
```

[/tab]

[/tabs]