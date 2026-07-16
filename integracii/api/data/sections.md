---
description: Ресурс Section — отдел с каталогами.
title: Разделы (Sections)
order: 1.5
searchPhrases:
  - получение разделов
  - получение раздела
  - создание раздела
  - изменение раздела
  - удаление раздела
---

Ресурс Section -- раздел с каталогами.

## Получить разделы

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/sections
```

Метод: **GET**

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
[{
    "id": "1",
    "icon": "icon1",
    "name": "My section"
},
{
    "id": "2",
    "icon": "icon2",
    "name": "Another section"
}]
```

[/tab]

[/tabs]

## Получить раздел

[tabs]

[tab:Запрос]

```javascript
URL: {domain}/api/v1/sections/{sectionId}
```

Метод: **GET**

Параметры:

-  `sectionId` (number) -- идентификатор отдела

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
[{
    "id": "1",
    "icon": "icon1",
    "name": "My section",
    "privilegeCode": "admin" // право на отдел
}]
```

[/tab]

[/tabs]

## Создать раздел

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/sections
```

Метод: **POST**\
\
Запрос: (application/json)

```javascript
{
    "name": "New section",
    "icon": "new-icon"
}
```

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
{
    "id": "2" // идентификатор созданного отдела
}
```

[/tab]

[/tabs]

## Изменить раздел

[tabs]

[tab:Запрос]

```javascript
URL: {domain}/api/v1/sections/{sectionId}
```

Метод: **PATCH**

Параметры:

-  `sectionId` (number) -- идентификатор отдела

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
{
    "name": "My section1",
    "catalogsPriorities": ["5", "3", "7"] // очередность каталогов в отделе
}
```

[/tab]

[/tabs]

## Удалить раздел

[tabs]

[tab:Запрос]

```javascript
URL: {domain}/api/v1/sections/{sectionId}
```

Метод: **DELETE**

Параметры:

-  `sectionId` (number) -- идентификатор отдела

[/tab]

[tab:Ответ]

Ответ: 200 OK

[/tab]

[/tabs]