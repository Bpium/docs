# Профиль (Profile/me)



## Получить пользователей

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/profile/me
```

Метод: **GET**
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "1", // идентификатор сотрудника в компании (в каталоге Сотрудники)
    "email": "mail@mail.ru", // почта или логин сотрудника
    "title": "Виктор", // имя сотрудника в компании
    "subjects": [ // список правовых групп сотрудника (из ресурса Права)
        {
            userAttr: "3",
            valueCatalogId: "57",
            valueRecordId: "4"
        },
        …
    ]
    
}
```
{% endtab %}
{% endtabs %}
