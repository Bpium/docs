# Получить запись

Используется для получения значений полей конкретной записи из Бипиума по её идентификатору. Процессы имеют доступ к записям минуя правовую политику.

## Свойства

### **Секция «Получить запись»**

**Указать каталог**\
Способ выбора каталога для получения записи. Доступные варианты: `из списка` и `через переменную`. Вариант «из списка» подойдет, когда вы знаете из какого каталога хотите получить запись. Вариант «через переменную» используется, если нужно получить запись из разных каталогов в зависимости от логики в сценарии.

**Каталог**\
Свойство доступно при выбранном значении `Указать каталог = Из списка`. Выбор каталога из числа доступных в системе для получения записи. Формат: выбор из списка каталогов.

**ID каталога**\
Свойство доступно при выбранном значении `Указать каталог = Через переменную`. Идентификатор (ID) каталога, из которого нужно получить запись. Формат: значение/выражение.

**ID записи**\
Идентификатор (ID) записи, которую нужно получить. Формат: значение/выражение.

### Секция **«Получить»**

**Поля**\
Способ указания какие поля записи должны вернуться в результате. Доступные варианты: `все поля` и `заданные`. Вариант «все поля» вернет все поля каталога, включая расширенные поля связанных записей, которые заданы в настройках полей каталога. Вариант «заданные» позволяет указать какие именно поля вернуть, в том числе и расширенные поля у связанных записей.

**ID полей**\
Свойство доступно при выбранном значении `Поля = Заданные`. Массив идентификаторов (API ID) полей каталога, указанных через запятую. Ограничение списка полей уменьшает размер данных в сценарии, и увеличивает скорость их получения. Также компонент позволяет получить любые расширенные поля у связанных записей. Формат: значение/выражение.

Формат указания [расширенных полей](../../catalog-edit.md#svyazannyi-katalog):

```javascript
[
  {
    fieldId: IDполя1,
    fields: {
      IDкаталога1: [ IDрасширенногоПоля1, IDрасширенногоПоля2, ... ],
      IDкаталога2: [ IDрасширенногоПоля3, IDрасширенногоПоля4, ... ],
      ...
    }
  },
  {
    fieldId: IDполя2,
    ...
  }
]
```

* `IDполяX` — поле выбранного каталога
* `IDкаталогаХ` — Идентификатор каталога связанного каталога. Поле «[связанные каталог](../../catalog-edit.md#svyazannyi-katalog)» может иметь несколько источников. Например каталог «Договор» в поле контрагент может ссылаться на каталоги «Клиентов», «Партнеров» и «Подрядчиков». Для каждого каталога источника можно указать какие именно поля требуется получить.
* `IDрасширенногоПоляХ` — идентификаторы полей связанного каталога, которые нужно получить.

{% hint style="info" %}
#### **Пример**

`[ 4, {fieldId: 5, fields: { 27: [2,6]} }, 8, {fieldId: 9, fields: { 19: [3,4]} } ]`

Получает 4, 5, 8 и 9 поля. Для 5 поля дополнительно возвращаются 2 и 6 поле у связанных записей из каталога 27, а для 9 поля — 3 и 4 поле у записей из каталога 19.
{% endhint %}

### **Секция «Результат»**

**Сохранить в**  \
Выходной параметр. Сохранит результат в указанную переменную. Формат: имя переменной.

```javascript
{
    id: '11',
    title: 'Название записи',
    values: {
        'field_id' : field_value,
        ...
    }
}
```

{% hint style="info" %}
В поле «**Сохранить в**» можно указать ключ объекта и результат сохранится как значения этого ключа.

**Пример**

Если указать в поле «**Сохранить в**» переменную`data.temp, то результат будет выглядеть следующим образом:`&#x20;

```javascript
data: {
    "temp": {
        id: '11',
        title: 'Название записи',
        values: {
            'field_id' : field_value,
            ...
        }
    }
}
```
{% endhint %}

## Формат значения полей (values)

Переменная с результатом («сохранить в») содержит объект values — значения полей записи. Ключи объекта — идентификаторы полей. Формат значений для разных типов полей разный:

* **Однострочный текст** = `"Однострочный текст"`
* **Многострочный текст** = `"Многострочный текст"`
* **Дата** = `"2015-11-06T21:00:00.000Z"`&#x20;
* **Категория / набор галочек** = `[2]` или несколько значений `[2,3,4]`, без значения `[]`
* **Вопрос** = `2`
* **Число** = `3.2`
* **Прогресс** = `28`(допустимо от 0 до 100)
* **Звезды** = `5` (допустимо от 0 до 5)
* **Контакт** = массив объектов `[{"contact": "8-901-234-56-78", comment: "Секретарь"}, {...}]`
* **Адрес** = объект предоставляемый из DaData

{% code fullWidth="false" %}
```json
{
            "value": "г Москва, Кремлевская наб",
            "provider": "dadata",
            "data": {
                "flat_type": null,
                "house": null,
                "city_kladr_id": "7700000000000",
                "postal_code": null,
                "area_with_type": null,
                "settlement_kladr_id": null,
                "street": "Кремлевская",
                "fias_id": "ede88e7f-8788-4851-a4d8-ff6d748f9bfe",
                "house_fias_id": null,
                "street_kladr_id": "77000000000160300",
                "region_with_type": "г Москва",
                "street_fias_id": "ede88e7f-8788-4851-a4d8-ff6d748f9bfe",
                "city": "Москва",
                "house_kladr_id": null,
                "kladr_id": "77000000000160300",
                "block_type": null,
                "block": null,
                "settlement_fias_id": null,
                "city_fias_id": "0c5b2444-70a0-4932-980c-b4dc0d3f02b5",
                "geo_lat": "55.748711",
                "area_fias_id": null,
                "country": "Россия",
                "region_kladr_id": "7700000000000",
                "street_with_type": "Кремлевская наб",
                "house_type": null,
                "flat": null,
                "region_fias_id": "0c5b2444-70a0-4932-980c-b4dc0d3f02b5",
                "geo_lon": "37.61697",
                "settlement_with_type": null,
                "area_kladr_id": null,
                "city_with_type": "г Москва"
            }
        }
```
{% endcode %}

* **Связанная запись** = массив объектов`[ {catalogId: '11',recordId: '91', catalogTitle: 'Название каталога', recordTitle: 'Название записи', isRemoved: false}, {...} ]`
* **Сотрудник** = массив объектов\
  `[{id: '21', title: 'Имя', isRemoved: false}, {...}]`
*   **Файл** = массив объектов

    `[ {title: "имя", url: "http://путь", size: 45654, mimeType: "image/png"}, {...} ]` \
    где `mimeType`— [допустимые значения](https://www.wikiwand.com/ru/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA\_MIME-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2)​

{% hint style="info" %}
**Как получить значение поля?**

Допустим, вы сохранили найденную запись в переменную `r`. Чтобы обратиться к коллекции значений полей, используйте выражение `r.values`.

Чтобы получить значение, например, 4-го поля записи, используйте компонент [назначение переменных](variables.md) и укажите выражение `r.values['4']`.

**Как получить значение сложного поля?**

Для сложных типов полей (значения которых представлены массивом, например: категория, связанный объект, контакт, сотрудник, файл) значение поля будет массивом. Чтобы узнать сколько элементов в массиве используйте выражение `r.values['4'].length`, а чтобы получить значение, например, 0-го элемента массива: `r.values['4'][0]`, чтобы обратится к его свойству, например _recordId_ у поля типа связанный объект: `r.values['4'][0].recordId`.

Однако, если вы обратитесь к параметру (например _recordId_) к элементу массива, которого нет (например, когда в поле не выбрано никакое значение), то сценарий прервется с ошибкой. Поэтому, прежде чем получить свойство у объекта в массиве, нужно сначала проверить что массив не пустой. Это можно сделать с помощью компонентов ветвления сценария (шлюзов) и проверки условий на исходящих соединительных линиях, или с помощью выражения: `r.values['4'].length && r.values['4'][0].recordId`.

Это выражение проверяет что в 4-м поле в массиве есть элементы, и если есть хотя бы один, то возвращает значение свойства _recordId_ в 0-м элементе. Именно это значение будет присвоено переменной. Если элементов нет, то переменной будет присвоено 0.
{% endhint %}

## Пограничные события

![](../../.gitbook/assets/boundary\_any.png)

Компонент поддерживает 2 типа пограничных событий:

* Ошибка — выход из компонента, если произошла какая-либо ошибка
* Таймаут — выход из компонента, спустя заданное ограничение по времени

Если компонент завершился с ошибкой, но на нем не было пограничного события, то процесс завершается. Сообщение ошибки возвращается в результатах процесса.
