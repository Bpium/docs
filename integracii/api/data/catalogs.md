---
description: Ресурс Catalog — каталог с записями.
title: Каталоги (Catalogs)
order: 1
searchPhrases:
  - список каталогов
  - получение каталога
  - права на каталог
  - поля каталога
  - создание каталога
  - изменение каталога
  - удаление каталога
---

Ресурс Catalog -- каталог с записями.

## Список каталогов

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs
```

Метод: **GET**

Возвращает список всех каталогов системы

Дополнительные параметры:

-  `sectionId` (строка) -- фильтр по отделу

-  `catalogId` (строка) -- фильтр по id каталога (доступно перечисление catalogId). Например:

```
URL: {domain}/api/v1/catalogs?id=23&24
```

Запрос вернет все указанные каталоги

[/tab]

[tab:Ответ]

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

[/tab]

[/tabs]

## Получить каталог

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}
    {?fields}
```

Метод: **GET**

Параметры:

-  `catalogId` (строка) -- идентификатор каталога

-  `fields` (json array, опционально) -- набор возвращаемых полей записей, формат: \["2", "3"\]. Доступно с версии API 1.9.1.

[/tab]

[tab:Ответ]

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

[/tab]

[/tabs]

## Создать каталог

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/
```

Метод: **POST**

Создание каталога в указанном разделе

Параметры запроса:

-  `sectionId` (строка) -- указание раздела, в котором создается каталог;

-  `name` (строка) -- наименование каталога;

-  `icon` (строка) -- иконка каталога (наименование иконки можно узнать при наведении на иконку в структуре каталога);

-  `fields` (массив) -- перечень полей каталога.

**Описание параметров для полей**

Каждый элемент массива `fields` описывает отдельное поле каталога.

{% table header="row" %}

---

*  Параметр

*  Тип

*  Описание

---

*  id

*  string

*  Идентификатор поля

---

*  prevId

*  string

*  Предыдущий идентификатор поля

---

*  duplicateResultWithPrevId

*  boolean

*  Дублировать данные с предыдущим полем

---

*  name (обязательный параметр)

*  string

*  Название поля

---

*  type (обязательный параметр)

*  string

*  Основной тип поля

   *описание возможных значений для type указаны в таблице ниже*

---

*  required

*  boolean

*  Обязательность заполнения

---

*  hint

*  string

*  Подсказка к заполнению

---

*  isSystem

*  boolean

*  Признак системного поля

---

*  history

*  boolean

*  Сохранять изменения поля в истории

---

*  filterable

*  boolean

*  Отображать поле в панели фильтров

---

*  apiOnly

*  boolean

*  Поле доступно только через API

---

*  hidden

*  boolean

*  true = поле скрыто

---

*  comment

*  string

*  Служебная заметка

---

*  visible

*  object

*  Настройка видимости поля

---

*  formulaConfig

*  object

*  Конфигурация формулы

   *примеры описания указаны в статье ниже*

---

*  formulaType

*  string

*  Определение типа формулы (lookup/rollup/formula/ai)

---

*  config

*  object

*  конфигурация поля (используется для полей, которые можно изменять в рамках типа поля. Например: текстовое поле можно указать текстовым редактором)

{% /table %}

**Описание возможных типов полей в параметре type**

| Тип поля    | Описание          |
|-------------|-------------------|
| group       | Секция            |
| text        | Текстовое поле    |
| number      | Число             |
| date        | Дата              |
| contact     | Контактные данные |
| address     | Адрес             |
| dropdown    | Статус            |
| checkboxes  | Набор галочек     |
| radiobutton | Выбор значения    |
| switch      | Переключатель     |
| progress    | Прогресс          |
| stars       | Оценка звездами   |
| user        | Сотрудник         |
| object      | Связанный каталог |
| file        | Файл              |
| iframe      | Веб-страница      |
| button      | Кнопка            |

:::info 

`type` определяет основной тип поля.

`config.type` определяет подтип или режим работы поля в рамках основного типа.

Дополнительные параметры и настройки поля задаются в объекте `config`. Например, для создания текстового редактора основной тип поля указывается как `text`, а в `config.type` -- значение `textEditor`.

:::

**Конфигурация полей**

`config` (объект) -- конфигурация поля. Примеры описания конфигурации по типам полей:

-  текстовое поле (text):

   -  ```json
      "type": "text",
      "config": {
                      "type": "text", //обычное текстовое поле
                      "mask": "111-111"
                  }
      ```

   -  ```json
      "type": "text",
      "config": {
                      "type": "textEditor", //текстовый редактор
                      "mask": null
                  }
      ```

   -  ```json
      "type": "text",
      "config": {
                      "type": "multiline", //многострочный текст
                      "mask": null
                  }
      ```

-  число (number):

   -  ```json
      "type": "number",
      "config": {
                      "unit": "",
                      "type": "number", //обычное числовое поле
                      "max": null,
                      "min": null
                  }
      ```

   -  ```json
      "type": "number",
      "config": {
                      "unit": "р.",
                      "type": "number", //числовое поле с доп параметрами
                      "min": "1000",
                      "max": "-1"
                  }
      ```

-  дата (date):

   -  ```json
      "type": "date",
      "config": {
                      "type": "date", //дата без времени
                      "time": false,
                      "defaultValue": false
                  }
      ```

   -  ```json
      "type": "date",
      "config": {
                      "type": "time", //дата с временем
                      "time": true,
                      "defaultValue": false
                  }
      ```

   -  ```json
      "type": "date",
      "config": {
                      "type": "week", //выбор недели
                      "time": false,
                      "defaultValue": false
                  }
      ```

   -  ```json
      "type": "date",
      "config": {
                      "type": "month", //выбор месяца
                      "time": false,
                      "defaultValue": false
                  }
      ```

   -  ```json
      "type": "date",
      "config": {
                      "type": "quarter", //выбор квартала
                      "time": false,
                      "defaultValue": false
                  }
      ```

   -  ```json
      "type": "date",
      "config": {
                      "type": "year", //выбор года
                      "time": false,
                      "defaultValue": false
                  }
      ```

-  контакт (contact):

   -  ```json
      "type": "contact",
      "config": {
                      "type": "phone", //телефон
                      "defaultEmptyValue": []
                  }
      ```

   -  ```json
      "type": "contact",
      "config": {
                      "type": "email", //электронная почта
                      "defaultEmptyValue": []
                  }
      ```

   -  ```json
      "type": "contact",
      "config": {
                      "type": "site", //сайт/ссылка
                      "defaultEmptyValue": []
                  }
      ```

-  адрес (address):

   -  ```json
      "type": "address",
      "config": {
                      "token": "",
                      "type": "address",
                      "defaultEmptyValue": {}
                  }
      ```

-  статус:

   -  ```json
      "type":"dropdown",
      "config": {
                      "items": [
                          {
                              "name": "Первое значение",
                              "color": "D3E3FF", //идентификатор цвета
                              "_cid": "генерируемое значение", //уникальный идентификатор значения статуса
                              "dbId": 1,
                              "id": "1"
                          },
                          {
                              "name": "Второе значение",
                              "color": "D3E3FF",
                              "_cid": "генерируемое значение",
                              "dbId": 2,
                              "id": "2"
                          }
                      ],
                      "multiselect": false, //возможность указания нескольких значений
                      "type": "dropdown",
                      "defaultEmptyValue": [
                          "1" //значение по умолчанию
                      ] 
                  }
      ```

-  набор галочек (checkboxes):

   -  ```json
      "type": "checkboxes",
      "config": {
                      "items": [
                          {
                              "name": "1",
                              "_cid": "генерируемое значение",
                              "dbId": 1,
                              "id": "1"
                          }
                      ],
                      "type": "checkboxes",
                      "defaultEmptyValue": []
                  }
      ```

-  выбор значения/выпадающий список (radiobutton):

   -  ```json
      "type": "radiobutton",
      "config": {
                      "items": [
                          {
                              "name": "1",
                              "_cid": "генерируемое значение",
                              "dbId": 1,
                              "id": "1"
                          }
                      ],
                      "type": "radiobuttonGroup" // выбор значения
                  }
      ```

   -  ```json
      "type": "radiobutton",
      "config": {
                      "items": [
                          {
                              "name": "1",
                              "_cid": "генерируемое значение",
                              "dbId": 1,
                              "id": "1"
                          }
                      ],
                      "type": "radiobuttonSelect" //выпадающий список
                  }
      ```

-  переключатель (switch):

   -  ```json
      "type": "switch",
      "config": {
                      "value": false,
                      "type": "switch",
                      "defaultValue": false
                  }
      ```

-  прогресс (progress):

   -  ```json
      "type": "progress",
      "config": {
                      "type": "progress"
                  }
      ```

-  оценка звездами (stars):

   -  ```json
      "type": "stars",
      "config": {
                      "type": "stars"
                  }
      ```

-  сотрудник (user):

   -  ```json
      "type": "user",
      "config": {
                      "multiselect": false,
                      "type": "user",
                      "defaultEmptyValue": []
                  }
      ```

-  связанный каталог (object):

   -  ```json
      "type": "object",
      "config": {
                      "multiselect": false, //можно связывать несколько записей
                      "accessOnly": false, //выбирать только из доступных
                      "enableCreate": true, //можно создавать новые записи
                      "enableUnsaved": false, //создание без всплывающего окна
                      "enableSelect": true, //можно выбирать из существующих
                      "mode": "list", //вид отображения list(список); cards(карточка); table(таблица)
                      "defaultEmptyValue": [],
                      "type": "object",
                      "catalogs": [
                          {
                              "dbId": 24,
                              "id": "24",
                              "title": "Наименование связанного каталога",
                              "icon": "content-11",
                              "removed": false
                          }
                      ],
                      "views": [], //указывается, если связь настраивается с видом каталога
                      "fields": {} //расширенные поля
                  }
      ```

-  файл (file):

   -  ```json
      "type": "file",
      "config": {
                      "multiselect": false,
                      "type": "file",
                      "defaultEmptyValue": []
                  }
      ```

-  веб-страница (iframe):

   -  ```json
      "type": "iframe",
      "config": {
                      "url": ""
                  }
      ```

**Примеры описания конфигурации формулы**

Формула:

```json
"formulaType": "formula",
"formulaConfig": {
                "formula": "2+1" 
            }
```

Подсчет по связям (rollup):

```json
"formulaType": "rollup",
"formulaConfig": {
                "fn": "sum", 
                "fieldId": "23",
                "linkCatalogId": "24",
                "linkFieldId": "3"
            }
```

Проброс по связям (lookup):

```json
"formulaType": "lookup",
"formulaConfig": {
                "sync": true,
                "fieldId": "23",
                "linkCatalogId": "24",
                "linkFieldId": "2"
            }
```

ИИ-поле:

```json
"formulaType": "ai",
"formulaConfig": {
                "service": "yandexGpt",
                "prompt": "123"
            }
```

**Запрос: (application/json)**

```javascript
{
    "name": "Каталог со всеми типами полей",
    "icon": "business-1",
    "sectionId": "16",
    "fields": [
        {
            "name": "Секция",
            "type": "group",
            "config": {
                "type": "text",
                "mask": null,
                "tab": true
            }
        },
        {
            "name": "Текст",
            "type": "text",
            "hint": "Обычное текстовое поле",
            "config": {
                "type": "text",
                "mask": null
            }
        },
        {
            "name": "Текстовый редактор",
            "type": "text",
            "config": {
                "type": "textEditor",
                "mask": null
            }
        },
        {
            "name": "Многострочный текст",
            "type": "text",
            "config": {
                "type": "multiline",
                "mask": null
            }
        },
        {
            "name": "Число",
            "type": "number",
            "config": {
                "unit": "",
                "type": "number",
                "min": null,
                "max": null
            }
        },
        {
            "name": "Число с ограничением",
            "type": "number",
            "config": {
                "unit": "р.",
                "type": "number",
                "min": "1000",
                "max": "100000"
            }
        },
        {
            "name": "Дата",
            "type": "date",
            "config": {
                "type": "date",
                "time": false,
                "defaultValue": false
            }
        },
        {
            "name": "Дата и время",
            "type": "date",
            "config": {
                "type": "time",
                "time": true,
                "defaultValue": false
            }
        },
        {
            "name": "Неделя",
            "type": "date",
            "config": {
                "type": "week",
                "time": false,
                "defaultValue": false
            }
        },
        {
            "name": "Месяц",
            "type": "date",
            "config": {
                "type": "month",
                "time": false,
                "defaultValue": false
            }
        },
        {
            "name": "Квартал",
            "type": "date",
            "config": {
                "type": "quarter",
                "time": false,
                "defaultValue": false
            }
        },
        {
            "name": "Год",
            "type": "date",
            "config": {
                "type": "year",
                "time": false,
                "defaultValue": false
            }
        },
        {
            "name": "Телефон",
            "type": "contact",
            "config": {
                "type": "phone",
                "defaultEmptyValue": []
            }
        },
        {
            "name": "Электронная почта",
            "type": "contact",
            "config": {
                "type": "email",
                "defaultEmptyValue": []
            }
        },
        {
            "name": "Сайт",
            "type": "contact",
            "config": {
                "type": "site",
                "defaultEmptyValue": []
            }
        },
        {
            "name": "Адрес",
            "type": "address",
            "config": {
                "token": "",
                "type": "address",
                "defaultEmptyValue": {}
            }
        },
        {
            "name": "Статус",
            "type": "dropdown",
            "config": {
                "items": [
                    {
                        "name": "Новый",
                        "color": "D3E3FF"
                    },
                    {
                        "name": "В работе",
                        "color": "D3E3FF"
                    },
                    {
                        "name": "Завершен",
                        "color": "D3E3FF"
                    }
                ],
                "multiselect": false,
                "type": "dropdown",
                "defaultEmptyValue": []
            }
        },
        {
            "name": "Набор галочек",
            "type": "checkboxes",
            "config": {
                "items": [
                    {
                        "name": "Первое значение"
                    },
                    {
                        "name": "Второе значение"
                    },
                    {
                        "name": "Третье значение"
                    }
                ],
                "type": "checkboxes",
                "defaultEmptyValue": []
            }
        },
        {
            "name": "Выбор значения",
            "type": "radiobutton",
            "config": {
                "items": [
                    {
                        "name": "Первое значение"
                    },
                    {
                        "name": "Второе значение"
                    }
                ],
                "type": "radiobuttonGroup"
            }
        },
        {
            "name": "Выпадающий список",
            "type": "radiobutton",
            "config": {
                "items": [
                    {
                        "name": "Первое значение"
                    },
                    {
                        "name": "Второе значение"
                    }
                ],
                "type": "radiobuttonSelect"
            }
        },
        {
            "name": "Переключатель",
            "type": "switch",
            "config": {
                "value": false,
                "type": "switch",
                "defaultValue": false
            }
        },
        {
            "name": "Прогресс",
            "type": "progress",
            "config": {
                "type": "progress"
            }
        },
        {
            "name": "Оценка звездами",
            "type": "stars",
            "config": {
                "type": "stars"
            }
        },
        {
            "name": "Сотрудник",
            "type": "user",
            "config": {
                "multiselect": false,
                "type": "user",
                "defaultEmptyValue": []
            }
        },
        {
            "name": "Связанный каталог",
            "type": "object",
            "config": {
                "multiselect": false,
                "accessOnly": false,
                "enableCreate": true,
                "enableUnsaved": false,
                "enableSelect": true,
                "mode": "list",
                "defaultEmptyValue": [],
                "type": "object",
                "catalogs": [
                    {
                        "id": "24"
                    }
                ],
                "views": [],
                "fields": {}
            }
        },
        {
            "name": "Файл",
            "type": "file",
            "config": {
                "multiselect": false,
                "type": "file",
                "defaultEmptyValue": []
            }
        },
        {
            "name": "Веб-страница",
            "type": "iframe",
            "config": {
                "url": ""
            }
        },
        {
            "name": "Кнопка",
            "type": "button",
            "config": {
                "items": [],
                "type": "button"
            }
        }
    ]
}
```

Возможные значения для icon описаны в [документации](http://okcss.dev.oktell.ru/#/elements/icons).

[/tab]

[tab:Ответ]

Ответ: 200 OK (application/json)

```javascript
{
    "id": "148",
    "sectionId": "16",
    "icon": "business-1",
    "name": "Каталог со всеми типами полей",
    "history": true,
    "manual": "",
    "fields": [
        {
            "id": "1",
            "prevId": "1",
            "duplicateResultWithPrevId": false,
            "name": "Секция",
            "required": false,
            "type": "group",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "text",
                "mask": null,
                "tab": true
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "2",
            "prevId": "2",
            "duplicateResultWithPrevId": false,
            "name": "Текст",
            "required": false,
            "type": "text",
            "hint": "Обычное текстовое поле",
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "text",
                "mask": null
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "3",
            "prevId": "3",
            "duplicateResultWithPrevId": false,
            "name": "Текстовый редактор",
            "required": false,
            "type": "text",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "textEditor",
                "mask": null
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "4",
            "prevId": "4",
            "duplicateResultWithPrevId": false,
            "name": "Многострочный текст",
            "required": false,
            "type": "text",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "multiline",
                "mask": null
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "5",
            "prevId": "5",
            "duplicateResultWithPrevId": false,
            "name": "Число",
            "required": false,
            "type": "number",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "unit": "",
                "type": "number",
                "min": null,
                "max": null
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "6",
            "prevId": "6",
            "duplicateResultWithPrevId": false,
            "name": "Число с ограничением",
            "required": false,
            "type": "number",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "unit": "р.",
                "type": "number",
                "min": "1000",
                "max": "100000"
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "7",
            "prevId": "7",
            "duplicateResultWithPrevId": false,
            "name": "Дата",
            "required": false,
            "type": "date",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "date",
                "time": false,
                "defaultValue": false
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "8",
            "prevId": "8",
            "duplicateResultWithPrevId": false,
            "name": "Дата и время",
            "required": false,
            "type": "date",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "time",
                "time": true,
                "defaultValue": false
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "9",
            "prevId": "9",
            "duplicateResultWithPrevId": false,
            "name": "Неделя",
            "required": false,
            "type": "date",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "week",
                "time": false,
                "defaultValue": false
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "10",
            "prevId": "10",
            "duplicateResultWithPrevId": false,
            "name": "Месяц",
            "required": false,
            "type": "date",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "month",
                "time": false,
                "defaultValue": false
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "11",
            "prevId": "11",
            "duplicateResultWithPrevId": false,
            "name": "Квартал",
            "required": false,
            "type": "date",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "quarter",
                "time": false,
                "defaultValue": false
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "12",
            "prevId": "12",
            "duplicateResultWithPrevId": false,
            "name": "Год",
            "required": false,
            "type": "date",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "year",
                "time": false,
                "defaultValue": false
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "13",
            "prevId": "13",
            "duplicateResultWithPrevId": false,
            "name": "Телефон",
            "required": false,
            "type": "contact",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "phone",
                "defaultEmptyValue": []
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "14",
            "prevId": "14",
            "duplicateResultWithPrevId": false,
            "name": "Электронная почта",
            "required": false,
            "type": "contact",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "email",
                "defaultEmptyValue": []
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "15",
            "prevId": "15",
            "duplicateResultWithPrevId": false,
            "name": "Сайт",
            "required": false,
            "type": "contact",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "site",
                "defaultEmptyValue": []
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "16",
            "prevId": "16",
            "duplicateResultWithPrevId": false,
            "name": "Адрес",
            "required": false,
            "type": "address",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "token": "",
                "type": "address",
                "defaultEmptyValue": {}
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "17",
            "prevId": "17",
            "duplicateResultWithPrevId": false,
            "name": "Статус",
            "required": false,
            "type": "dropdown",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "items": [
                    {
                        "name": "Новый",
                        "color": "D3E3FF",
                        "dbId": 1,
                        "id": "1"
                    },
                    {
                        "name": "В работе",
                        "color": "D3E3FF",
                        "dbId": 2,
                        "id": "2"
                    },
                    {
                        "name": "Завершен",
                        "color": "D3E3FF",
                        "dbId": 3,
                        "id": "3"
                    }
                ],
                "multiselect": false,
                "type": "dropdown",
                "defaultEmptyValue": []
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "18",
            "prevId": "18",
            "duplicateResultWithPrevId": false,
            "name": "Набор галочек",
            "required": false,
            "type": "checkboxes",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "items": [
                    {
                        "name": "Первое значение",
                        "dbId": 1,
                        "id": "1"
                    },
                    {
                        "name": "Второе значение",
                        "dbId": 2,
                        "id": "2"
                    },
                    {
                        "name": "Третье значение",
                        "dbId": 3,
                        "id": "3"
                    }
                ],
                "type": "checkboxes",
                "defaultEmptyValue": []
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "19",
            "prevId": "19",
            "duplicateResultWithPrevId": false,
            "name": "Выбор значения",
            "required": false,
            "type": "radiobutton",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "items": [
                    {
                        "name": "Первое значение",
                        "dbId": 1,
                        "id": "1"
                    },
                    {
                        "name": "Второе значение",
                        "dbId": 2,
                        "id": "2"
                    }
                ],
                "type": "radiobuttonGroup"
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "20",
            "prevId": "20",
            "duplicateResultWithPrevId": false,
            "name": "Выпадающий список",
            "required": false,
            "type": "radiobutton",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "items": [
                    {
                        "name": "Первое значение",
                        "dbId": 1,
                        "id": "1"
                    },
                    {
                        "name": "Второе значение",
                        "dbId": 2,
                        "id": "2"
                    }
                ],
                "type": "radiobuttonSelect"
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "21",
            "prevId": "21",
            "duplicateResultWithPrevId": false,
            "name": "Переключатель",
            "required": false,
            "type": "switch",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "value": false,
                "type": "switch",
                "defaultValue": false
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "22",
            "prevId": "22",
            "duplicateResultWithPrevId": false,
            "name": "Прогресс",
            "required": false,
            "type": "progress",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "progress"
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "23",
            "prevId": "23",
            "duplicateResultWithPrevId": false,
            "name": "Оценка звездами",
            "required": false,
            "type": "stars",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "type": "stars"
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "24",
            "prevId": "24",
            "duplicateResultWithPrevId": false,
            "name": "Сотрудник",
            "required": false,
            "type": "user",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "type": "user",
                "defaultEmptyValue": []
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "25",
            "prevId": "25",
            "duplicateResultWithPrevId": false,
            "name": "Связанный каталог",
            "required": false,
            "type": "object",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "accessOnly": false,
                "enableCreate": true,
                "enableUnsaved": false,
                "enableSelect": true,
                "mode": "list",
                "defaultEmptyValue": [],
                "type": "object",
                "catalogs": [
                    {
                        "dbId": 24,
                        "id": "24",
                        "title": "Веб форма",
                        "icon": "content-11",
                        "removed": false
                    }
                ],
                "views": [],
                "fields": {}
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "26",
            "prevId": "26",
            "duplicateResultWithPrevId": false,
            "name": "Файл",
            "required": false,
            "type": "file",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "type": "file",
                "defaultEmptyValue": []
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "27",
            "prevId": "27",
            "duplicateResultWithPrevId": false,
            "name": "Веб-страница",
            "required": false,
            "type": "iframe",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "url": ""
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        },
        {
            "id": "28",
            "prevId": "28",
            "duplicateResultWithPrevId": false,
            "name": "Кнопка",
            "required": false,
            "type": "button",
            "hint": null,
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "hidden": false,
            "comment": "",
            "config": {
                "items": [],
                "type": "button"
            },
            "visible": null,
            "formulaConfig": null,
            "formulaType": null
        }
    ],
    "privilegeCode": "admin",
    "fieldPrivilegeCodes": {}
}
```

[/tab]

[/tabs]

## Изменить каталог

:::info 

ВАЖНО! При изменении структуры каталога, если в теле запроса не передать ранее заданные поля, они будут удалены.

:::

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}
```

Метод: **PATCH**

Параметры:

-  `catalogId` (строка) -- идентификатор каталога

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

[/tab]

[tab:Ответ]

Ответ: 200 OK

```json
{
    "id": "141" //id измененного каталога
}
```

[/tab]

[/tabs]

## Удалить каталог

[tabs]

[tab:Запрос]

```
URL: {domain}/api/v1/catalogs/{catalogId}
```

Метод: **DELETE**

Параметры:

-  `catalogId` (строка) -- идентификатор каталога

[/tab]

[tab:Ответ]

Ответ: 200 OK

[/tab]

[/tabs]