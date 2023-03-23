---
description: Ресурс Relations - хранит связи между записями.
---

# Связи (Relations)

## Получить связи

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/records/{recordId}/relations
```

Метод: **GET**

Параметры:

* `catalogId` (number) — идентификатор каталога
* `recordId` (number) — идентификатор записи

Фильтры поиска:

* `catalogId` (number) - связи с конкретным каталогом
* `fieldId` (number) - связи по полю записи

Можно так же применить фильтры из ["Получить записи"](../../../api-records.md#poluchit-zapisi).
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[{
    "id": "1",
    "sectionId": "1",
    "icon": "content-1",
    "title": "Название",
    "privilegeCode": "admin",
    "fieldId": "2",
    "fieldName": "Документы",
    "records": [{
            "id": "1",
            "title": "Название связанног объекта",
            "created": "2019-08-02T12:21:58.421Z"
    }],
    "recordsTotal": 1
}]
```
{% endtab %}
{% endtabs %}
