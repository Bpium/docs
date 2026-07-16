---
description: Ресурс Histories - хранит историю изменений записей в каталоге.
title: Истории (Histories)
order: 0.9
searchPhrases:
  - история изменений
  - ресурс Histories
  - получение истории
  - активность каталога
  - фильтр по пользователю
---

Ресурс Histories - хранит историю изменений записей в каталоге.

## Получить историю

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/histories?catalogId={catalogId}&recordId={recordId}
```

Метод: **GET**

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `recordId` (number, опционально) -- если не указать - вернет историю по каталогу - [Активность](../../../../rabota-v-bipiume/records/operacii/activity)

Параметры для коллекции записей:

-  `limit` (number, опционально) - количество историй для получения

-  `from` (number, опционально) - аналогично `offset`

-  `sortType` (string, опционально) - порядок по: возрастанию(`asc`), убыванию(`desc`, по-умолчанию)

-  `userId` (number, опционально) - фильтр по пользователю

Если не указан `recordId` - можно применить фильтры из ["Получить записи"](./records#poluchit-zapisi).

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
[{
    "id": "249108",
    "catalogId": "2",
    "recordId": "1",
    "recordTitle": "Название",
    "actionType": "CREATE",
    "payload": {
        "2": {
            "oldValue": "",
            "newValue": "Счет на участие в выставке"
        }
    },
    "date": "2019-08-02T12:21:55.364Z",
    "user": {
        "id": "1",
        "title": "Admin"
    }
}]
```

[/tab]

[/tabs]