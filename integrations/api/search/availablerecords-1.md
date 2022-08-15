# Доступные условия фильтра (AvailableFilterRecords)

## Получить доступные связи

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/fields/{fieldId}/availableFilterRecords
    {?title}
```

Метод: **GET**

Параметры:

* `catalogId` (string) — текущий каталог
* `fieldId` (string) — поле каталога с выпадающим списком

Фильтры поиска:

* `title` (string) — поисковая строка для фильтрации связанных записей по введенному значению
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```
[
    {
        "sectionId": "1",
        "catalogId": "4",
        "catalogTitle": "Каталог",
        "catalogIcon": "transfers-52",
        "recordId": "15",
        "recordTitle": "Клиенты"
    },
    {
        "sectionId": "2",
        "catalogId": "4",
        "catalogTitle": "Каталог",
        "catalogIcon": "transfers-52",
        "recordId": "26",
        "recordTitle": "Клиенты"
    }
]
```
{% endtab %}
{% endtabs %}
