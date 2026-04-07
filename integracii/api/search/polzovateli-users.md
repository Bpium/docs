---
description: Ресурс Users - хранит данные о пользователях системы.
---

# Сотрудники (Users)

## Получить пользователей

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/users
```

Метод: **GET**
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[
    {
        "id": "1",
        "title": "Администратор"
    },
    {
        "id": "2",
        "title": "Руководитель"
    },
    {
        "id": "3",
        "title": "Сотрудник"
    }
]
```
{% endtab %}
{% endtabs %}
