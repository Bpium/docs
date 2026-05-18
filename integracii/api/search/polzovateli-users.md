---
description: Ресурс Users - хранит данные о пользователях системы.
title: Сотрудники (Users)
order: 4
---

Ресурс Users - хранит данные о пользователях системы.

## Получить пользователей

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/users
```

Метод: **GET**

[/tab]

[tab:Ответ]

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

[/tab]

[/tabs]