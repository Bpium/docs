---
description: Ресурс Section — отдел с каталогами.
---

# Отделы (Sections)

## Получить отделы

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/sections
```

Метод: **GET**
{% endtab %}

{% tab title="Ответ" %}
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
{% endtab %}
{% endtabs %}

## Получить отдел

{% tabs %}
{% tab title="Запрос" %}
```javascript
URL: {domain}/api/v1/sections/{sectionId}
```

Метод: **GET**

Параметры:

* `sectionId` (number) — идентификатор отдела
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[{
    "id": "1",
    "icon": "icon1",
    "name": "My section",
    "privilegeCode": "admin" // право на отдел
}]
```
{% endtab %}
{% endtabs %}

## Создать отдел

{% tabs %}
{% tab title="Запрос" %}
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

Возможные значения для icon описаны в [документации](http://okcss.dev.oktell.ru/#/elements/icons).
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "2" // идентификатор созданного отдела
}
```
{% endtab %}
{% endtabs %}

## Изменить отдел

{% tabs %}
{% tab title="Запрос" %}
```javascript
URL: {domain}/api/v1/sections/{sectionId}
```

Метод: **PATCH**

Параметры:

* `sectionId` (number) — идентификатор отдела
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "name": "My section1",
    "catalogsPriorities": ["5", "3", "7"] // очередность каталогов в отделе
}
```
{% endtab %}
{% endtabs %}

## Удалить отдел

{% tabs %}
{% tab title="Запрос" %}
```javascript
URL: {domain}/api/v1/sections/{sectionId}
```

Метод: **DELETE**

Параметры:

* `sectionId` (number) — идентификатор отдела
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK
{% endtab %}
{% endtabs %}
