# Доступные связи (AvailableRecords)

## Получить доступные связи

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/fields/{fieldId}/availableRecords
    {?title}
    {?recordsFilters}
```

Метод: **GET**

Параметры:

* `catalogId` (string) — текущий каталог
* `fieldId` (string) — поле каталога с выпадающим списком

Фильтры поиска:

* `title` (string) — поисковая строка для фильтрации связанных записей по введенному значению
* `catalogId` (string) — источник (каталог) связанных записей, используется если с полем связано несколько каталогов и нужно показать доступные для связывания записи только из одного из них.
* `recordsFilters` (string) — сериализованный JSON-объект с дополнительными фильтрами по связанным каталогам. Используется для фильтрования доступных для связывания записей по их свойствам. Например применяется для зависимых связанных полей:

```
recordsFilters = [
  {
    "catalogId": "id связанного каталога",
    "viewId: "id связанного вида",
    "filters": [
      {
        "fieldId": "id поля c данными для сравнения",
        "value": значение
      },
      ...
    ]
  },
  ...
]
```
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
        "recordTitle": "Клиенты",
        "recordValues": {}
    },
    {
        "sectionId": "2",
        "catalogId": "4",
        "catalogTitle": "Каталог",
        "catalogIcon": "transfers-52",
        "recordId": "26",
        "recordTitle": "Клиенты",
        "recordValues": {}
    }
]
```
{% endtab %}
{% endtabs %}
