---
description: "Свока —\_суммарное подсчитанное значение по записям каталога"
title: Сводка (Totals)
order: 1
---

Сводка -- суммарное подсчитанное значение по записям каталога

:::info 

Ресурс Totals доступен начиная с версии Бипиума 1.7.1.\
В предыдущих версиях Бипиума значения разложения можно получить через ресурс [Widget/Totals](./../reports/widgets).

:::

## Получить сводку

[tabs]

[tab:Запрос]

```
URL: /{api url}/catalogs/{catalogId}/totals
            ?value[type]=recordsCount
            &axis[type]=field
            &axis[value]=2
            &recordsFilter[filters][0][fieldId]=12
            &recordsFilter[filters][0][value][0]=CURRENT_USER
```

Метод: **GET**

Параметры:

-  `catalogId` (string) -- идентификатор каталога

Параметры фильтра (определяют выборку):

-  *Параметры аналогичны параметрам получения значений разложения* [*Values*](./values)*.*\
   *Игнорируются параметры: sort, sortType, limit, offset.*

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
[{
        "key": "sum",
        "value": 1
    },
    {
        "key": "avg",
        "value": 1
    },
    {
        "key": "count",
        "value": 1
}]
```

[/tab]

[/tabs]