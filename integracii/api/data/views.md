---
description: Ресурс View — вид (сохраненный фильтр) каталога.
title: Виды (Views)
order: 0.5
searchPhrases:
  - сохраненный фильтр
  - получение видов
  - правовой вид
  - личный вид
  - идентификатор вида
  - создание вида
  - изменение вида
  - удаление вида
---

Ресурс View представляет собой сохраненный вид каталога. Вид содержит настройки фильтрации записей и/или разметки каталога и может быть личным, общим или правовым.

## Получить виды

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/views
```

Метод: **GET**

Метод возвращает список всех доступных видов указанного каталога.

Параметры:

-  `catalogId` (number) -- идентификатор каталога

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
[
{
    "id": "58",
    "catalogId": "125",
    "catalogTitle": "Наименование каталога",
    "catalogIcon": "content-11",
    "name": "Наименование вида",
    "type": "shared",
    "forRights": true,
    "originName": "Наименование вида для администратора",
    "viewMode": "table",
    "privilegesApi": {
        "value": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ]
    },
    "privilegeCode": "admin",
    "filters": [
        {
            "id": 38,
            "fieldId": "4",
            "value": [
                "3"
            ]
        }
    ]
},
{
//Второй вид
}
]
```

[/tab]

[/tabs]

## Получить вид

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/views/{viewId}
```

Метод: **GET**

Метод возвращает информацию о выбранном виде, включая его настройки фильтрации.

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `viewId` (number) -- идентификатор вида

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
{
    "id": "58",
    "catalogId": "125",
    "catalogTitle": "Наименование каталога",
    "catalogIcon": "content-11",
    "name": "Наименование вида",
    "type": "shared",
    "forRights": true,
    "originName": "Наименование вида для администратора",
    "viewMode": "table",
    "privilegesApi": {
        "value": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ]
    },
    "privilegeCode": "admin",
    "filters": [
        {
            "id": 38,
            "fieldId": "4",
            "value": [
                "3"
            ]
        }
    ]
}
```

Filters -- массив фильтров. Каждый фильтр это объект, состоящий из параметра attr (указывает на ID поля для фильтрации данных) и объекта value (параметры поискового запроса). Value для разных типов полей имеет разную структуру.

Формат параметра value для разных типов полей:

-  Для текстовых полей -- поиск по вхождению: value = ""

-  Для дат, чисел, прогресса -- поиск по диапазону: value = \{ at : '...', to : '...' }

-  Для категории, набора галочек, вопроса, звёзд -- поиск по вхождению: value = \[1,2,3,5\]

-  Для связанных объектов: value = \[ \{ catalogId:18, recordId:9 }, \{ catalogId:18, recordId:10 } \]

-  Для сотрудников: value = \[21, 22, 'CURRENT_USER'\]

[/tab]

[/tabs]

## Создать вид

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/views
```

Метод: **POST**

Метод для создания новых видов. Поддерживается создание личных, общих и правовых видов.

Параметры:

-  `catalogId` (number) -- идентификатор каталога

Обязательные параметры тела запроса:

-  `name` - наименование вида;

-  `type` - тип вида (обязателен, если не указан параметр `forRights`). Возможные значения:

   -  shared - правовой;

   -  personal - личный;

   -  global - общий.

-  `forRights` - признак правового вида (обязателен, если не указан параметр `type`). Возможные значения:

   -  true - создание правового вида;

   -  false - при отсутствии параметра type, по умолчанию создается личный вид.

Запрос: (application/json)

```javascript
{
    "name": "View public name",
    "originName": "View name for admins", //в случае отсутствия параметра, будет использовано значение name
    "type": "shared", //правовой вид, global - общий, personal - личный
	"forRights": true, // true — правовой вид, false — личный вид
    "filters": [
        {
            "fieldId": "13",
            "attr": "13",
            "value": ["1", "2", "5"]
        },
        {
            "fieldId": "12",
            "attr": "12",
            "value": {
                  "at": "2015-10-27T00:00:00+03:00",
                  "to" : "2015-11-19T23:59:59+03:00"
            }
        }
    ]
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

## Изменить вид

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/views/{viewId}
```

Метод: **PATCH**

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `viewId` (number) -- идентификатор вида

Запрос: (application/json)

```javascript
{
    "name": "View public name",
    "originName": "View name for admins",
	"type": "personal", //возможно изменение типа вида
    "forRights": false,
    "filters": [
        {
            "attr": "12",
            "value": ["1", "2", "5"]
        },
        {
            "attr": "13",
            "value": {
                  "at": "2015-10-27T00:00:00+03:00",
                  "to" : "2015-11-19T23:59:59+03:00"
            }
        }
}
```

[/tab]

[tab:Ответ]

Ответ: 200 ОК

```javascript
{
    "id": "7" // идентификатор измененного вида
}
```

[/tab]

[/tabs]

## Удалить вид

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}/views/{viewId}
```

Метод: **DELETE**

Параметры:

-  `catalogId` (number) -- идентификатор каталога

-  `viewId` (number) -- идентификатор вида

[/tab]

[tab:Ответ]

Ответ: 200 ОК

[/tab]

[/tabs]