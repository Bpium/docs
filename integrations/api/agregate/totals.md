---
description: "Свока —\_суммарное подсчитанное значение по записям каталога"
---

# Сводка (Totals)

{% hint style="info" %}
Ресурс Totals доступен начиная с версии Бипиума 1.7.1.\
В предыдущих версиях Бипиума значения разложения можно получить через ресурс [Widget/Totals](../reports/widgets.md#obshie-dannye-grafika-totals).
{% endhint %}

## Получить сводку

{% tabs %}
{% tab title="Запрос" %}
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

* `catalogId` (string) — идентификатор каталога

Параметры фильтра (определяют выборку):

* _Параметры аналогичны параметрам получения значений разложения_ [_Values_](values.md)_._\
  _Игнорируются параметры: sort, sortType, limit, offset._

Естественным языком о смысле параметров описано в статье про [интерфейс графика](../../../dashboard-widgets.md).
{% endtab %}

{% tab title="Ответ" %}
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
{% endtab %}
{% endtabs %}
