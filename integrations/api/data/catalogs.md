---
description: Ресурс Catalog — каталог с записями.
---

# Каталоги (Catalogs)

## Список каталогов

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs?sectionId={sectionId}
```

Метод: **GET**

Параметры:

* `sectionId` (строка) — фильтр по отделу
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[{
    "id": "1",
    "sectionId": "1",
    "icon": "user-2",
    "name": "My catalog",
    "fieldPrivilegeCodes": {
        "9": "edit",
        "22": "edit"
    }
},
{
    "id": "2",
    "sectionId": "2",
    "icon": "user-3",
    "name": "Another catalog",
    "fieldPrivilegeCodes": {}
}]
```
{% endtab %}
{% endtabs %}

## Получить каталог

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}
    {?fields}
```

Метод: **GET**

Параметры:

* `catalogId` (строка) — идентификатор каталога
* `fields` (json array, опционально) — набор возвращаемых полей записей, формат: \["2", "3"]. Доступно с версии API 1.9.1.
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "1",
    "sectionId": "1",
    "icon": "icon1",
    "name": "My catalog",
    "privilegeCode": "admin", // право на каталог
    "fieldPrivilegeCodes": { // права на поля для пользователя от имени которого пришел запрос
        "9": "edit",
        "22": "edit"
    },
    "fields": [
        {
            "id": "1",
            "name": "User",
            "type": "group"
        },
        {
            "id": "2",
            "name": "Username",
            "type": "text",
            "config": {
                "type": "mail"
            }
        },
        {
            "id": "3",
            "name": "Birthday",
            "type": "date",
            "config": {
                "time": false
            },
        {
            "id": "28",
            "name": "Связанный каталог",
            "type": "object",
            "hint": "",
            "required": false,
            "apiOnly": false,
            "config": {
                "multiselect": true,
                "accessOnly": false,
                "catalogs": [
                    {
                        "id": "25",
                        "title": "Связанный каталог",
                        "icon": "business-23",
                        "removed": false
                    }
                ],
                "views": [],
                "fields": {
                    "25": [
                        {
                            "id": "2",
                            "name": "Число",
                            "type": "number",
                            "hint": "",
                            "required": false,
                            "apiOnly": false,
                            "config": {
                                "unit": "",
                                "defaultEmptyValue": null
                            }
                        },
                        {
                            "id": "3",
                            "name": "Дата",
                            "type": "date",
                            "hint": "",
                            "required": false,
                            "apiOnly": false,
                            "config": {
                                "time": false,
                                "defaultValue": false
                            }
                        },
                        {
                            "id": "4",
                            "name": "Связанный каталог",
                            "type": "object",
                            "hint": "",
                            "required": false,
                            "apiOnly": false,
                            "config": {
                                "catalogs": [],
                                "views": [],
                                "defaultEmptyValue": [],
                                "fields": {}
                            }
                        }
                    ]
                },
                "defaultEmptyValue": []
            }
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## Создать каталог

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/
```

Метод: **POST**

Запрос: (application/json)

```javascript
{
    "name": "New catalog",
    "icon": "icon",
    "sectionId": "2",
    "fields": [{
        "name": "Секция",
        "hint": "",
        "type": "group",
        "config": {}
    }, {
        "name": "Текст",
        "hint": "Подсказка к полю текст",
        "type": "text",
        "config": {
            "type": "text"
        }
    }, {
        "name": "Дата",
        "hint": "",
        "type": "date",
        "config": {
            "time": false,
            "notificationField": null
        }
    }, {
        "name": "Набор галочек",
        "hint": "",
        "type": "checkboxes",
        "config": {
            "items": [{
                "name": "1"
            }, {
                "name": "2"
            }, {
                "name": "3"
            }]
        }
    }, {
        "name": "Прогресс",
        "hint": "",
        "type": "progress",
        "config": {}
    }, {
        "name": "Сотрудник",
        "hint": "",
        "type": "user",
        "config": {
            "multiselect": false
        }
    }, {
        "name": "Связанный объект",
        "hint": "",
        "type": "object",
        "config": {
            "multiselect": false,
            "catalogs": [{
                "id": "11"
            }]
        }
    }, {
        "name": "Файл",
        "hint": "",
        "type": "file",
        "config": {
            "multiselect": false
        }
    }]
}
```

Возможные значения для icon описаны в [документации](http://okcss.dev.oktell.ru/#/elements/icons).
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "3" // идентификатор созданного каталога
    "values": {
        // значения полей аналогично получению каталога
    }
}
```
{% endtab %}
{% endtabs %}

## Изменить каталог

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}
```

Метод: **PATCH**

Параметры:

* `catalogId` (строка) — идентификатор каталога

Запрос: (application/json)

```javascript
{
    "name": "My catalog1",
    // если не передать параметр (например icon), то он изменен не будет
    "fields" : [
        {
            "id": "1", // чтобы сохранить существующее поле, нужно указать его id
            "name": "User",
            "type": "group"
        },
        {
            "id": "2",
            "name": "User full name", // в существующем поле можно изменить имя
            "type": "text", // тип заменить нельзя
            "config": { // в существующем поле можно изменить его параметры
                "type": "mail"
            }
        },

        // поле 3 в новом наборе полей не передали: если оно было, оно будет удалено

        // создали новое поле
        {
            "name": "Age",
            "type": "number"
        }
    ]
}
```
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK
{% endtab %}
{% endtabs %}

## Удалить каталог

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/catalogs/{catalogId}
```

Метод: **DELETE**

Параметры:

* `catalogId` (строка) — идентификатор каталога
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK
{% endtab %}
{% endtabs %}
