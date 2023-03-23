---
description: >-
  Ресурс Widgets — графическое отображение распределения записей в каталоге по
  выбранным критериям.
---

# Графики (Widgets)

### Получить графики

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards/{boardsId}/widgets
```

Метод: **GET**

Параметры:

* `boardsId` (number) — идентификатор дашборда
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
[
   {
       "id":"14",
       "name":null,
       "chartType":"pie",
       "stacked":null,
       "value":
           {
               "value":"8",
               "type":"field",
               "subType":"timeLeft"
           },
       "valueFn":"avg",
       "axis":
           {
               "value":"12",
               "type":"field",
               "subType":null
           },
       "split":null,
       "recordsType":"available",
       "recordsFilter":null
   },
   {
       "id":"15",
       "name":null,
       "chartType":"columns",
       "stacked":null,
       "value":
           {
               "value":"",
               "type":"recordsCount",
               "subType":null
           },
       "valueFn":null,
       "axis":
           {
               "value":"2",
               "type":"field",
               "subType":null
           },
       "split":
           {
               "value":"2",
               "type":"field",
               "subType":null
           },
       "recordsType":"all",
       "recordsFilter":null
   }
]
```
{% endtab %}
{% endtabs %}

### Получить график

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards/{boardsId}/widgets/{widgetsId}
```

Метод: **GET**

Параметры:

* `boardsId` (number) — идентификатор дашборда
* `widgetsId` (number) — идентификатор графика
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "14",
    "name": null,
    "chartType": "pie", // тип графика
    "stacked": null, // сложить значения
    "value": // поле 'Значение'
    {
        "type": "field", // тип параметра в поле 'Значение'
        "value": "8", // id поля
        "subType": "timeLeft" // обязательный параметр для полей типа: категория, набор галочек, вопрос
    },
    "valueFn": "avg", // значение функции
    "axis": // поле 'Ось'
    {
        "type": "field",
        "value": "12",
        "subType": null // обязательный параметр для даты создания
    },
    "split": null, // поле 'Разложить по' (недоступно для графика типа pie)
    "recordsType": "available", // доступные записи
    "recordsFilter": // фильтр
    {
        "filters": {
            "4": // id поля для фильтра
            [{
                    "recordId": "5",
                    "catalogId": "65"
                }
            ]
        }
    }
}
```

**Возможные значения:**

* chartType: columns (столбцы), lines (линия), bars (список), pie (пирог), radar (радар), number (число)
* stacked: true и false
* value\[type]: recordsCount (количество записей), field (поле)
* value\[value]: id поля (если value\[type]=field)
* value\[subType]: timeLeft (продолжительность)
* valueFn: sum (сумма), avg (среднее, не выбранные значения не учитываются), avgAll (среднее по всем, не выбранные значения считаются за ноль), max (максимальное), min (минимальное)
* axis\[type]: field (поле), createdTime (дата создания)
* axis\[value]: id поля (если axis\[type]=field)
*   axis\[subType]: hour (час), hourOfDay (час дня), day (день), dayOfWeek (день месяца), week (неделя), weekOfYear (неделя года), month (месяц), monthOfYear (месяц года), year (год) &#x20;

    Подробнее про [Ось](https://docs.bpium.ru/dashboard-widgets.html#%D0%BE%D1%81%D1%8C). &#x20;
* split: возможны такие же значения как в axis
* recordsType: all (все записи каталога), available (только доступные сотруднику записи)
{% endtab %}
{% endtabs %}

### Создать график

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards/{boardsId}/widgets
```

Метод: **POST**

Параметры:

* `boardsId` (number) — идентификатор дашборда

Запрос: (application/json)

```javascript
        {
            "chartType":"columns",
            "value":
                {
                    "type":"recordsCount",
                    "value":null
                },
            "valueFn":null,
            "axis":
                {
                    "type":"field",
                    "value":"4"
                },
            "recordsType":"all"
        }
```
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

```javascript
{
    "id": "16" // идентификатор созданного графика
}
```
{% endtab %}
{% endtabs %}

### Изменить график

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards/{boardsId}/widgets/{widgetsId}
```

Метод: **PATCH**

Параметры:

* `boardsId` (number) — идентификатор дашборда
* `widgetsId` (number) — идентификатор графика

Запрос: (application/json)

```javascript
{
    "chartType":"columns"
}
```
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)
{% endtab %}
{% endtabs %}

### Удалить график

{% tabs %}
{% tab title="Запрос" %}
```
URL: {domain}/api/v1/boards/{boardsId}/widgets/{widgetsId}
```

Метод: **DELETE**

Параметры:

* `boardsId` (number) — идентификатор дашборда
* `widgetsId` (number) — идентификатор графика
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK
{% endtab %}
{% endtabs %}

## Значения графика (Values)

{% tabs %}
{% tab title="Запрос" %}
### Получить значения распределения графика

```
URL: {domain}/api/v1/boards/{boardsId}/widgets/{widgetsId}/values
            ?recordsFilter[filters][0][fieldId]=12
            &recordsFilter[filters][0][value][0]=CURRENT_USER
            &limit=100
            &offset=0
```

Метод: **GET**

Параметры:

* `boardsId` (number) — идентификатор дашборда
* `widgetsId` (number) — идентификатор графика

Параметры фильтра (определяют выборку):

* `value[type]` (string) — тип подсчитываемого значения
  * `recordsCount` — количество найденных записей (по умолчанию)
  * `field` — значение по полю каталога
* `value[value]` (string) — идентификатор поля каталога для расчета. Допустимо указывать поля типов: число, прогресс, звезды, статус, набор галочек, выбор, связанный объект.
* `value[subType]` (string) — подтип подсчитываемого значения
  * `uniqueCount` — подсчет уникальных значений (доступно для полей типа связанная запись, сотрудник, текст)
  * `timeLeft` — время нахождения записей в определенном значении поля (доступно для полей статус, набор галочек, выбор). Например: сколько записи были в определенных статусах.
  * `timeLeft` — время до момента перехода в в определенное значении поля (доступно для полей статус, набор галочек, выбор). Например: сколько прошло времени до того как записи перешли в определенный статус.
* `valueFn` — применяемая функция для подсчета значений
  * `''` (пусто) — не применять никакую функцию (для value\[type]=recordsCount)
  * `count` — количество записей (значений)
  * `sum` — сумма значений
  * `max` — максимальное значение в группе
  * `min` — минимальное значение в группе
  * `avg` — среднее значение в группе (пустые значения не учитываются)
  * `avgAll` — среднее значение в группе (пустые значения считаются за 0)
* `axis[type]` (string) — тип оси для разложения записей:
  * `all` — без разложения (значение по умолчанию)
  * `field` — разложение по полю каталога
  * `createdTime` — разложение по времени создания записи
* `axis[value]` (string) — идентификатор поля оси разложения (для axis\[type]=field)
* `axis[subType]` (string) — тип группировки данных по оси (для полей типа дата)
  * `hour` — по часам
  * `hourOfDay` — по часам дня (все дни «схлопываются»)
  * `day` — по дням
  * `dayOfWeek` — по дням недели (пн—вс)
  * `week` — по неделям
  * `weekOfYear` — по неделям года (все года «схлопываются»)
  * `monthOfYear` — по месяцам года (все года «схлопываются»)
  * `month` — по месяцам
  * `year` — по годам
* `split[type]` (string) — тип разделения (ось 2) значений внутри разложения по оси
  * возможные значения аналогичны параметру `axis[type]`
* `split[value]` (string) — идентификатор поля оси 2 (для split\[type]=field)
* `split[subType]` (string) — тип группировки данных по оси 2 (для полей типа дата)
  * возможные значения аналогичны параметру `axis[subType]`
* `splitLimit` (число) — если по оси2 вариантов разложения много, то берутся не все, а только часть их них, те которые встречаются чаще (по умолчанию 10, максимум 100)
* `sort` (string) — тип сортировки полученной выборки. Для разных типов полей значение по умолчанию разное.
  * `axis` — сортировать по наименованию (порядку) заголовков осей.
    * Для дат — в порядке их следования
    * Для статусов, наборов галочек, выбора — в порядке указания элементов в поле каталога
    * Для текста, связанных записей, сотрудников — по наименованию
  * `value` — сортировать по величине значений
* `sortType` (string) — направление сортировки полученной выборки
  * `asc` — по алфавиту / в порядке хронологии / в порядке указания в каталоге
  * `desc` — в обратном порядке
* `recordsType` (string) — какие записи учитывать для подсчета значений
  * `all` — все записи каталога без проверки прав (доступно, если у сотрудника, делающего запрос есть права на просмотр всех записей каталога)
  * `available` — учитывать только доступные сотруднику записи каталога
* `recordsFilter` (object) — фильтры для выборки записей каталога, такие как filter, sort и другие. подробнее в описании параметров [получения записей](api-records.md#poluchit-zapisi).
* `limit` (number) — количество возвращаемых разложений (по умолчанию 30, максимум 1000)
* `offset` (number) — смещение получаемой выборки разложения относительно начала, используется для постраничного получения данных с параметром limit

Естественным языком о смысле параметров описано в статье про [интерфейс графика](dashboard-widgets.md).



### Получить значения распределения без создания графика

В версии Бипиума 1.7.1 появился специальный ресурс [Values](integrations/api/agregate/values.md) (используйте его).\
В более ранних версиях используйте метод получения через существующий Дашборд:

```
URL: /{api url}/boards/{boardsId}/widgets/new/values
            ?value[type]=recordsCount
            &axis[type]=field
            &axis[value]=2
            &recordsFilter[filters][0][fieldId]=12
            &recordsFilter[filters][0][value][0]=CURRENT_USER
            &limit=100
            &offset=0
```

Возможные параметры запроса можете найти в параграфе [График (Widgets)](https://docs.bpium.ru/reports/widgets)

Метод: **GET**

Параметры:

* `boardsId` (number) — идентификатор дашборда

Параметры фильтра (определяют выборку):

* _параметры аналогичны параметрам получения значений графика_ [_Values_](api-widget.md#znacheniya-grafika-values)__
{% endtab %}

{% tab title="Ответ" %}
Ответ: 200 OK (application/json)

Пример с наличием разложения по оси (axis):

```javascript
[
{
    "axis": "Анатолий",
    "values": [
        {
            "value": "7"
        }
    ]
},
{
    "axis": "Виктор",
    "values": [
        {
            "value": "4"
        }
    ]
}
]
```

Пример с наличием разложения по оси (axis) и оси 2 (split):

```javascript
[
{
    "axis": "Анатолий",
    "values": [
        {
            "split": "1"
            "value": "3"
        },
        {
            "split": "2"
            "value": "4"
        }
    ]
},
{
    "axis": "Виктор",
    "values": [
        {
            "split": "2"
            "value": "3"
        },
        {
            "split": "3"
            "value": "1"
        }
    ]
}
]
```

Параметры:

* `axis` — группа распределения, значение может быть строкой или объектом (в зависимости от оси разложения)
* `values` (array of objects) — набор значений
  * `split` — группа разделения (ось 2), значение может быть строкой или объектом (в зависимости от оси разложения)
  * `value` (number) — значение в группе (ось) с учетом подгруппы если есть (ось 2)
{% endtab %}
{% endtabs %}

## Общие данные графика (Totals)

{% tabs %}
{% tab title="Запрос" %}
### Получить общие данные графика

```
URL: /{api url}/boards/{boardsId}/widgets/{widgetsId}/totals
            ?value[type]=recordsCount
            &axis[type]=field
            &axis[value]=2
            &recordsFilter[filters][0][fieldId]=12
            &recordsFilter[filters][0][value][0]=CURRENT_USER
```

Метод: **GET**

Параметры:

* `boardsId` (number) — идентификатор дашборда
* `widgetsId` (number) — идентификатор графика

Параметры фильтра (определяют выборку):

* _Параметры аналогичны параметрам получения значений разложения_ [_Values_](integrations/api/agregate/values.md)_._\
  _Игнорируются параметры: sort, sortType, limit, offset._

__

### Получить общие данные без создания графика

В версии Бипиума 1.7.1 появился специальный ресурс [Totals](integrations/api/agregate/totals.md) (используйте его).\
В более ранних версиях используйте метод получения через существующий Дашборд:

```
URL: /{api url}/boards/{boardsId}/widgets/new/totals
            ?value[type]=recordsCount
            &axis[type]=field
            &axis[value]=2
            &recordsFilter[filters][0][fieldId]=12
            &recordsFilter[filters][0][value][0]=CURRENT_USER
```

Метод: **GET**

Параметры:

* `boardsId` (number) — идентификатор дашборда

Параметры фильтра (определяют выборку):

* _Параметры аналогичны параметрам получения значений разложения_ [_Values_](integrations/api/agregate/values.md)_._\
  _Игнорируются параметры: sort, sortType, limit, offset._
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
