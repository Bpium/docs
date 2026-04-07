---
description: Ресурс View — вид (сохраненный фильтр) каталога.
---

# Виды (Views)

## Получить виды

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/views
```

Метод: **GET**

Параметры:

* `catalogId` (number) — идентификатор каталога
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[
    {
        "id": "5",
        "name": "View with Rights",
        "originName": "View for juniors",
        "forRights": true
    },
    {
        "id": "6",
        "name": "My own view",
        "forRights": false
    }
]
```
{% endtab %}
{% endtabs %}

## Получить вид

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/views/{viewId}
```

Метод: **GET**

Параметры:

* `catalogId` (number) — идентификатор каталога
* `viewId` (number) — идентификатор вида
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "4",
    "name": "View public name",
    "originName": "View name for admins",
    "forRights": true, // true — правовой вид, false — личный вид
    "privilegeCode": "access" // право на вид
    "filters": [
        {
            "id": "101", // идентификатор условия
            "attr": "12", // идентификатор поля каталога для фильтрации
            "value": ["1", "2", "5"] // условия фильтрации
        },
        {
            "id": "102",
            "attr": "13",
            "value": {
                  "at": "2015-10-27T00:00:00+03:00",
                  "to" : "2015-11-19T23:59:59+03:00"
            }
        }
    ]
}
```

Filters — массив фильтров. Каждый фильтр это объект, состоящий из параметра attr (указывает на ID поля для фильтрации данных) и объекта value (параметры поискового запроса). Value для разных типов полей имеет разную структуру.

Формат параметра value для разных типов полей:

* Для текстовых полей — поиск по вхождению: value = ""
* Для дат, чисел, прогресса — поиск по диапазону: value = { at : '...', to : '...' }
* Для категории, набора галочек, вопроса, звёзд — поиск по вхождению: value = \[1,2,3,5]
* Для связанных объектов: value = \[ { catalogId:18, recordId:9 }, { catalogId:18, recordId:10 } ]
* Для сотрудников: value = \[21, 22, 'CURRENT\_USER']
{% endtab %}
{% endtabs %}

## Создать вид

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/views
```

Метод: **POST**

Параметры:

* `catalogId` (number) — идентификатор каталога

Запрос: (application/json)

```javascript
{
    "name": "View public name",
    "originName": "View name for admins",
    "forRights": true, // true — правовой вид, false — личный вид
    "filters": [
        {
            "id": "101",
            "attr": "12",
            "value": ["1", "2", "5"]
        },
        {
            "id": "102",
            "attr": "13",
            "value": {
                  "at": "2015-10-27T00:00:00+03:00",
                  "to" : "2015-11-19T23:59:59+03:00"
            }
        }
    ]
}
```
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "7" // идентификатор созданного вида
}
```
{% endtab %}
{% endtabs %}

## Изменить вид

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/views/{viewId}
```

Метод: **PATCH**

Параметры:

* `catalogId` (number) — идентификатор каталога
* `viewId` (number) — идентификатор вида

Запрос: (application/json)

```javascript
{
    "name": "View public name",
    "originName": "View name for admins",
    "forRights": true,
    "filters": [
        {
            "id": "101",
            "attr": "12",
            "value": ["1", "2", "5"]
        },
        {
            "id": "102",
            "attr": "13",
            "value": {
                  "at": "2015-10-27T00:00:00+03:00",
                  "to" : "2015-11-19T23:59:59+03:00"
            }
        }
}
```
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 ОК
{% endtab %}
{% endtabs %}

## Удалить вид

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}/views/{viewId}
```

Метод: **DELETE**

Параметры:

* `catalogId` (number) — идентификатор каталога
* `viewId` (number) — идентификатор вида
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 ОК
{% endtab %}
{% endtabs %}
