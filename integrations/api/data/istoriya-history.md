---
description: Ресурс Histories - хранит историю изменений записей в каталоге.
---

# Истории (Histories)

## Получить историю

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/histories?catalogId={catalogId}&recordId={recordId}
```

Метод: **GET**

Параметры:

* `catalogId` (number) — идентификатор каталога
* `recordId` (number, опционально) — если не указать - вернет историю по каталогу - [Активность](../../../manual/structure/catalogs/activity.md)

Параметры для коллекции записей:

* `limit` (number, опционально) - количество историй для получения
* `from` (number, опционально) - аналогично `offset`
* `sortType` (string, опционально) - порядок по: возрастанию(`asc`), убыванию(`desc`, по-умолчанию)
* `userId` (number, опционально) - фильтр по пользователю

Если не указан `recordId` - можно применить фильтры из ["Получить записи"](records.md#poluchit-zapisi).
{% endtab %}

{% tab title="Ответ" %}
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
{% endtab %}
{% endtabs %}

## Написать комментарий

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/histories
```

Метод: **POST**

Запрос: (application/json)

```
{
    "catalogId": "1",
    "recordId": "2",
    "type": "COMMENT",
    "payload": {
        "message": "Тест"
    }
}
```
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```
{
    "id": "2603083" // id коментария
}
```
{% endtab %}
{% endtabs %}

