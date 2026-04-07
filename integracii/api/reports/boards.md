---
description: Ресурс Boards — дашборд с графиками.
---

# Дашборды (Boards)

## Получить дашборды

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards{?catalogId}{?viewId}
```

Метод: **GET**

Параметры:

* `catalogId` (number, опционально) — идентификатор каталога
* `viewId` (number, опционально) — идентификатор вида
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[{
    "id": "5",
    "name": null,
    "catalogId": "64",
    "viewId": null
}, {
    "id": "6",
    "name": null,
    "catalogId": "64",
    "viewId": null
}, {
    "id": "8",
    "name": null,
    "catalogId": "64",
    "viewId": "142"
}, {
    "id": "9",
    "name": null,
    "catalogId": "64",
    "viewId": "137"
}, {
    "id": "7",
    "name": null,
    "catalogId": "64",
    "viewId": "140"
}]
```
{% endtab %}
{% endtabs %}

## Получить дашборд

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards/{boardsId}
```

Метод: **GET**

Параметры:

* `boardsId` (number) — идентификатор дашборда
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[{
    "id": "6",
    "name": null,
    "catalogId": "64",
    "viewId": null,
    "layouts": {
        "xs": { // параметры отображения графика на широком экране
            "14": { // идентификатор графика
                "x": 0,
                "y": 0,
                "w": 2,
                "h": 10
            }
        },
        "xxs": { // параметры отображения графика на узком экране
            "14": {
                "x": 0,
                "y": 0,
                "w": 1,
                "h": 4
            }
        }
    }
}]
```
{% endtab %}
{% endtabs %}

## Создать дашборд

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards
```

Метод: **POST**

Пример запроса: (application/json)

```javascript
    {
        "catalogId": "64"
    }
```
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "6" // идентификатор созданного дашборда
}
```
{% endtab %}
{% endtabs %}

## Удалить дашборд

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards/{boardsId}
```

Метод: **DELETE**

Параметры:

* `boardsId` (number) — идентификатор дашборда
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)
{% endtab %}
{% endtabs %}
