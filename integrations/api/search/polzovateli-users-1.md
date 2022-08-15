---
description: Ресурс Users - хранит данные о пользователях системы.
---

# Контакты (Contacts)

## Получить записи с контактами

{% tabs %}
{% tab title="Запрос" %}


```
URL: /{api url}/contacts
      {?searchText}
      {?sortField}{?sortType}
      {?limit}{?offset}
```

Метод: **GET**

Параметры:

* `searchText` (string, опционально) — быстрый поиск по вхождению
* `sortField` (number, опционально) — идентификатор поля для сортировки
* `sortType` (number, опционально) — тип сортировки
  * `1` — по возрастанию (по умолчанию)
  * `-1` — по убыванию
* `limit` (number, опционально) - количество возвращаемых записей (по умолчанию: 100)
* `offset` (number, опционально) — смещение от начала списка (по умолчанию: 0)
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[
    {
        "sectionId": "1",
        "catalogId": "14",
        "catalogTitle": "Клиенты",
        "catalogIcon": "transfers-50",
        "recordId": "15",
        "recordTitle": "ООО «Клиентище»",
        "recordValues": {
            "7": "8-987-654-32-10" // поле и значение найденного контакта
        }
    },
    {
        "sectionId": "2",
        "catalogId": "15",
        "catalogTitle": "Партнеры",
        "catalogIcon": "users-10",
        "recordId": "26",
        "recordTitle": "ООО «Партнерище»",
        "recordValues": {
            "10": "8-901-234-56-78" // поле и значение найденного контакта
        }
    }
]
```
{% endtab %}
{% endtabs %}
