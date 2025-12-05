# ❗ Обновление до версии 2.0

{% hint style="warning" %}
Данная инструкция обязательна к изучению в случаях, когда в системе работают автоматизации, настроены вебхуки и/или выполняются внешние взаимодействия с данными системы по API. В других случаях обновление не окажет влияния на работу системы и интеграций. В данной статье описаны ключевые аспекты обновления, по которым можно определить влияние изменений на конкретную систему.
{% endhint %}

## Необратимые изменения в версии 2.0

### Для облачной и серверной версий

1. Изменен ID служебного раздела «Управление» с числового на текстовый. Раздел теперь недоступен по числовому ID.
2. Изменены ID служебных каталогов («Сотрудники», «События» и др.) в разделе «Управление» с числовых на текстовые. Каталоги теперь доступны как по числовому, так и по текстовому ID.
3. Изменены ID полей служебных и серверных каталогов с числовых на текстовые. Ресурсы Catalogs (каталоги) и Records (записи) поддерживают работу в обоих форматах ID полей. Все другие ресурсы работают только в новом текстовом формате ID полей (возвращают данные и принимают параметры).

### Для серверной версии

1. Изменен ID служебного раздела «Система» с числового на текстовый. Раздел больше недоступен по числовому ID.
2. Изменены ID серверных каталогов («Домены», «Аккаунты» и др.) в разделе «Система» с числовых на текстовые. Каталоги теперь доступны как по числовому, так и по текстовому ID.
3. Изменена структура БД: переименованы имена колонок во многих таблицах (подробности ниже).



## Как понять, что изменения повлияют на работу системы

### 1. Работа с записями служебных каталогов

{% hint style="info" %}
Если вы работаете с записями служебных каталогов по API, то в качестве ID записей и полей могут использоваться как текстовые, так и числовые ID.
{% endhint %}

#### **\[Records.Get и Records.Find]**

При получении/поиске записей служебных каталогов можно указывать как текстовый, так и числовой ID каталога. Для поиска записей, в массиве фильтров в атрибутах _fieldId_, _catalogId_ и _recordId_ может указываться как текстовый, так и числовой ID. В возвращаемых данных атрибут _catalogId_ будет указан в текстовом формате, а поля записей будут задублированы (будут в числовом и текстовом форматах).

<details>

<summary>Пример: Получение записи из служебного каталога «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs/3/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)&#x20;

```json
{
    "id": "1",
    "catalogId": "3", // Числовой ID каталога «Сотрудники»
    "title": "Александр",
    "values": { // ID полей в числовом формате
        "1": "Александр",
        "2": "email@email.ru",
    },
    "chat": {
        "messagesCount": 0,
        "newMessages": false,
        "subscribe": false
    }
}
```

***

_Версия Бипиум 2.0_

_URL_ \[GET]**:** [https://\[домен\]/api/v1/catalogs/3/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)&#x20;

_URL_ \[GET]: [https://\[домен\]/api/v1/catalogs/$users/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)&#x20;

```json
{
    "id": "1",
    "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
    "title": "Александр",
    "values": { // ID полей в числовом и текстовом форматах
        "1": "Александр",
        "2": "email@email.ru",
        "$name": "Александр", 
        "$email": "email@email.ru"
    },
    "chat": {
        "messagesCount": 0,
        "newMessages": false,
        "subscribe": false
    },
}
```

</details>

***

#### \[Records.Create и Records.Update]

При создании/изменении записей по API можно указывать как текстовые, так и числовые ID каталогов, записей и полей  (в том числе и для полей связанных записей).

<details>

<summary>Пример: Создание записи в служебном каталоге «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[POST]**:** [https://\[ домен \]/api/v1/catalogs/3/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)&#x20;

```json
{
    "values": {
        "1": "Александр",
        "2": "email@email.ru",
        "3" [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "catalogId": "3", // Числовой ID каталога «Сотрудники»
                "recordId": "7"
            }
        ]
    }
}
```

***

_Версия Бипиум 2.0_

_URL_ \[POST]**:** [https://\[домен\]/api/v1/catalogs/3/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)&#x20;

```json
{
    "values": {
        "1": "Александр",
        "2": "email@email.ru",
        "3" [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "catalogId": "3", // Числовой ID каталога «Сотрудники»
                "recordId": "7"
            }
        ]
    }
}
```

***

_URL_ \[POST]**:** [https://\[домен\]/api/v1/catalogs/$users/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)&#x20;

```json
{
    "values": {
        "$name": "Александр",
        "$email": "email@email.ru"
        "3" [ // Поле типа «Связанный каталог», которое ссылается на каталог "Сотрудники"
            {
                "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
                "recordId": "7"
            }
        ]
    }
}
```



</details>

***

#### \[Входные данные сценариев]

Во входных параметрах атрибут _catalogId_ будет иметь текстовый формат. Поля записи в переменных _values_, _prevValues_ и _allValues_ будут задублированы (будут в числовом и текстовом форматах).

<details>

<summary>Пример: Входные данные сценариев при создании записи в служебном каталоге «Сотрудники»</summary>

_Версия Бипиум 1.\*_

<pre class="language-json"><code class="lang-json"><strong>{
</strong>    "id": "1",
    "catalogId": "3", // Числовой ID каталога «Сотрудники»
    "title": "Александр",
    "allValues": { // ID полей в числовом формате
        "1": "Александр",
        "2": "email@email.ru"
    }
    "prevValues": {
        "1": "Александр",
        "2": "email@email.ru"
    },
    "values": {
        "1": "Александр",
        "2": "email@email.ru"
    }
}
</code></pre>

***

_Версия Бипиум 2.0_

```json
{
    "id": "1",
    "catalogId": "$users",
    "title": "Александр",
    "allValues": { // ID полей в числовом и текстовом форматах
        "1": "Александр",
        "2": "email@email.ru",
        "$name": "Александр",
        "$email": "email@email.ru"
    }
    "prevValues": {
        "1": "Александр",
        "2": "email@email.ru",
        "$name": "Александр",
        "$email": "email@email.ru"
    },
    "values": {
        "1": "Александр",
        "2": "email@email.ru",
        "$name": "Александр",
        "$email": "email@email.ru"
    }
}
```

</details>

***

#### \[Вебхуки]

В данных вебхуков атрибут _catalogId_ будет иметь текстовый формат. Поля атрибутов _values_, _prevValues_ и _allValues_ будут задублированы (будут в числовом и текстовом форматах)

<details>

<summary>Пример: Данные вебхуков при создании записи в служебном каталоге «Сотрудники»</summary>

_Версия Бипиум 1.\*_

<pre class="language-json"><code class="lang-json"><strong>{
</strong>    "id": "2",
    "catalogId": "3", // Числовой ID каталога «Сотрудники»
    "title": "Александр",
    "allValues": { // ID полей в числовом формате
        "1": "Александр",
        "2": "email@email.ru",
    }
    "prevValues": {
        "1": "Александр",
        "2": "email@email.ru",
    },
    "values": {
        "1": "Александр",
        "2": "email@email.ru",
    }
}
</code></pre>

***

_Версия Бипиум 2.0_

```json
{
    "id": "2",
    "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
    "title": "Александр",
    "allValues": { // ID полей в числовом и текстовом форматах
        "1": "Александр",
        "2": "email@email.ru",
        "$name": "Александр",
        "$email": "email@email.ru"
    }
    "prevValues": {
        "1": "Александр",
        "2": "email@email.ru",
        "$name": "Александр",
        "$email": "email@email.ru"
    },
    "values": {
        "1": "Александр",
        "2": "email@email.ru",
        "$name": "Александр",
        "$email": "email@email.ru"
    }
}
```

</details>

### 2. Работа с записями любых каталог со связями на служебные каталоги

{% hint style="info" %}
Если вы используете в своих каталогах поля типа «Связанный каталог», которые ссылаются на служебные каталоги, связанные записи будут приходить с текстовыми ID каталогов.
{% endhint %}

#### \[Records.Get и Records.Find]

При получении записей по API, в атрибуте _values_ для полей типа «Связанный каталог», которые ссылаются на служебные каталоги, атрибуты _catalogId_ и _sectionId_ будут указаны в текстовом формате.&#x20;

<details>

<summary>Пример: Получение записи пользовательского каталога, который имеет связь со служебным каталогом «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs/\[ числовой id каталога \]/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)

```json
{
    "id": "1",
    "catalogId": "28",
    "title": "Просто текст",
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4": [
            "1"
        ],
        "6": [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "sectionId": "1", // Числовой ID раздела «Управление»
                "catalogId": "3", // Числовой ID каталога «Сотрудники»
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    },
    "chat": {
        "messagesCount": 0,
        "newMessages": false,
        "subscribe": false
    }
}
```

***

_Версия Бипиум 2.0_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs/\[ текстовый id каталога \]/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)

```json
{
    "id": "1",
    "catalogId": "28",
    "title": "Просто текст",
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4": [
            "1"
        ],
        "6": [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "sectionId": "$settings", // Текстовый ID раздела «Управление»
                "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    },
    "chat": {
        "messagesCount": 0,
        "newMessages": false,
        "subscribe": false
    }
}
```

</details>

***

#### \[Records.Create и Records.Update]

При создании/изменении записей можно указывать атрибут _catalogId_ в любом формате. В возвращаемых данных _catalogId_ будет также в текстовом формате, а поля будут задублированы.

<details>

<summary>Пример: Создание записи в пользовательском каталоге, который имеет связь со служебным каталогом «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL \[POST]_**:** [https://\[ домен \]/api/v1/catalogs/\[ числовой id каталога \]/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)

```json
{
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4" [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "catalogId": "3", // Числовой ID каталога «Сотрудники»
                "recordId": "7"
            }
        ]
    }
}
```

***

_Версия Бипиум 2.0_

_URL \[POST]_**:** [https://\[ домен \]/api/v1/catalogs/\[ числовой id каталога \]/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)

```json
{
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4" [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "catalogId": "3", // Числовой ID каталога «Сотрудники»
                "recordId": "7"
            }
        ]
    }
}
```

***

_URL \[POST]_**:** [https://\[домен\]/api/v1/catalogs/\[ текстовый id каталога \]/records](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)&#x20;

```json
{
    "values": {
        "2": "Александр",
        "3": "email@email.ru"
        "4" [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
                "recordId": "7"
            }
        ]
    }
}
```

</details>

***

#### \[Входные данные сценариев]

Во входных параметрах сценариев, при создании/изменении записей в связях по служебным каталогам атрибут _catalogId_ будет указан в текстовом формате.

<details>

<summary>Пример: Входные данные сценариев при событии создания записи в пользовательском каталоге, который имеет связь со служебным каталогом «Сотрудники»</summary>

_Версия Бипиум 1.\*_

```json
{
    "id": "4",
    "catalogId": "28",
    "title": "Просто текст",
    "allValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "sectionId": "1", // Числовой ID раздела «Управление»
                "catalogId": "3", // Числовой ID каталога «Сотрудники»
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    }
    "prevValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [ 
            {
                "sectionId": "1",
                "catalogId": "3",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    },
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4": [ 
            {
                "sectionId": "1",
                "catalogId": "3",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    }
}
```

***

_Версия Бипиум 2.0_

```json
{
    "id": "4",
    "catalogId": "28",
    "title": "Просто текст",
    "allValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудник
            {
                "sectionId": "$settings", // Текстовый ID раздела «Управление»
                "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    }
    "prevValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [ 
            {
                "sectionId": "$settings",
                "catalogId": "$users",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    },
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4": [ 
            {
                "sectionId": "$settings",
                "catalogId": "$users",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    }
}    
```

</details>

***

#### \[Вебхуки]

В вебхуках в связях по служебным каталогам атрибут _catalogId_ будет указан в текстовом формате.

<details>

<summary>Пример: Данные вебхуков при создании записи в пользовательском каталоге, который имеет связь со служебным каталогом «Сотрудники»</summary>

_Версия Бипиум 1.\*_

```json
{
    "id": "2",
    "catalogId": "28",
    "title": "Просто текст",
    "allValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "sectionId": "1", // Числовой ID раздела «Управление»
                "catalogId": "3", // Числовой ID каталога «Сотрудники»
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    }
    "prevValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [ 
            {
                "sectionId": "1",
                "catalogId": "3",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ] 
    },
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4": [
            {
                "sectionId": "1",
                "catalogId": "3",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ] 
    }
}
```

***

_Версия Бипиум 2.0_

```json
{
    "id": "2",
    "catalogId": "28",
    "title": "Просто текст",
    "allValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [ // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            {
                "sectionId": "$settings", // Текстовый ID раздела «Управление»
                "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ]
    }
    "prevValues": {
        "2": "Просто текст",
        "3": 99,
        "4": [
            {
                "sectionId": "$settings",
                "catalogId": "$users",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ] 
    },
    "values": {
        "2": "Просто текст",
        "3": 99,
        "4": [
            {
                "sectionId": "$settings",
                "catalogId": "$users",
                "catalogTitle": "Сотрудники",
                "catalogIcon": "users-1",
                "recordId": "1",
                "recordTitle": "Александр",
                "isRemoved": false
            }
        ] 
    }
}
```

</details>

***

#### \[Views.Get]

При получении определенного вида каталога в поле _value_ атрибута _filters_ для полей типа «Связанный каталог», которые ссылаются на служебные каталоги, будут указаны текстовые значения атрибута _catalogId_.

<details>

<summary>Пример: Получение вида каталога, который имеет связь со служебным каталогом «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]_:_ https://\[ домен ]/api/v1/catalogs/\[ id каталога ]/views/\[ id вида ]

```json
{
  [
    {
        "id": "20",
        "catalogId": "127",
        "catalogTitle": "Тестовый каталог",
        "catalogIcon": "content-11",
        "name": "Мои записи",
        "forRights": true,
        "originName": "Мои записи",
        "privilegesApi": {
            "value": [
                "admin",
                "access",
                "delete",
                "export",
                "create",
                "edit",
                "view",
                "search",
                "available"
            ]
        },
        "privilegeCode": "admin",
        "filters": [
            {
                "id": "30",
                "fieldId": "3",
                "value": [
                    {
                        "catalogId": "3", // Числовой ID каталога «Сотрудники»
                        "recordTitle": "Александр",
                        "recordId": "3",
                        "catalogIcon": "users-1",
                        "catalogDbId": 3,
                        "recordDbId": 3
                    }
                ]
            }
        ]
    }
]
```

***

_Версия Бипиум 2.0_

<pre class="language-json"><code class="lang-json"><strong>{
</strong>  [
    {
        "id": "20",
        "catalogId": "127",
        "catalogTitle": "Тестовый каталог",
        "catalogIcon": "content-11",
        "name": "Мои записи",
        "forRights": true,
        "originName": "Мои записи",
        "privilegesApi": {
            "value": [
                "admin",
                "access",
                "delete",
                "export",
                "create",
                "edit",
                "view",
                "search",
                "available"
            ]
        },
        "privilegeCode": "admin",
        "filters": [
            {
                "id": "30",
                "fieldId": "3",
                "value": [
                    {
                        "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
                        "recordTitle": "Александр",
                        "recordId": "3",
                        "catalogIcon": "users-1",
                        "catalogDbId": 3,
                        "recordDbId": 3
                    }
                ]
            }
        ]
    }
]
</code></pre>

</details>



### 3. Работа со структурами любых каталогов

{% hint style="info" %}
Если вы делаете запросы к API на получение, создание или изменение структуры каталога в сценариях или сторонних сервисах, то возвращаемые данные могут иметь задублированные поля.
{% endhint %}

#### \[Catalogs.Get, Catalogs.Create и Catalogs.Update]

В возвращаемых данных в объекте _fields_ для полей типа «Связанный каталог» будут перечислены текстовые ID каталогов, если эти каталоги являются служебными. В ином случае ID каталога может иметь числовое значение, если, например, оно было задано системой по-умолчанию. При этом, для служебных каталогов, будут указаны задублированные поля - в числовом и текстовом форматах.

<details>

<summary>Пример: Получение структуры пользовательского каталога, который имеет связь со служебным каталогом «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs/](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)\[ числовой id каталога ]&#x20;

```json
{
    "id": "141",
    "sectionId": "2",
    "icon": "content-11",
    "chat": {
        "newChats": 0
    },
    "name": "Каталог",
    "history": true,
    "fields": [
        {
            "id": "4", // Поле типа «Связанный каталог», которое ссылается на каталог «Сотрудники»
            "name": "Сотрудник",
            "required": false,
            "type": "object",
            "hint": "",
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "accessOnly": false,
                "enableCreate": true,
                "enableUnsaved": false,
                "enableSelect": true,
                "mode": "cards",
                "defaultEmptyValue": [],
                "type": "object",
                "catalogs": [
                    {
                        "id": "3", // Числовой ID каталога
                        "title": "Сотрудники",
                        "icon": "users-1",
                        "removed": false
                    }
                ],
                "views": [],
                "fields": {
                    "3": [ // Числовой ID каталога
                        {
                            "id": "3", // Числовой ID каталога
                            "name": "Имя",
                            "columnName": "name",
                            "required": false,
                            "type": "text",
                            "hint": "",
                            "position": 1,
                            "isSystem": true,
                            "history": true,
                            "filterable": true,
                            "apiOnly": false,
                            "comment": "",
                            "config": {
                                "type": "text",
                                "mask": ""
                            },
                            "visible": {},
                            "createdAt": "2023-11-13T07:57:03.782Z",
                            "updatedAt": "2023-11-13T07:57:03.782Z",
                            "_catalogId": "3"
                        },
                        {
                            "id": "2", // Числовой ID каталога
                            "name": "Эл. почта",
                            "columnName": "email",
                            "required": false,
                            "type": "text",
                            "hint": "На указанный адрес будет отправлено приглашение на вход в систему",
                            "position": 2,
                            "isSystem": true,
                            "history": true,
                            "filterable": true,
                            "apiOnly": false,
                            "comment": "",
                            "config": {
                                "type": "mail",
                                "mask": ""
                            },
                            "visible": {},
                            "createdAt": "2023-11-13T07:57:03.782Z",
                            "updatedAt": "2023-11-13T07:57:03.782Z",
                            "_catalogId": "3"
                        }
                    ]
                }
            },
            "visible": {}
        }
    ],
    "privilegeCode": "admin",
    "fieldPrivilegeCodes": {}
}

```

***

_Версия Бипиум 2.0_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs/](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)\[ числовой или текстовый id каталога ]&#x20;

```json
{
    "id": "141",
    "sectionId": "2",
    "icon": "content-11",
    "chat": {
        "newChats": 0
    },
    "name": "Каталог",
    "history": true,
    "fields": [
        {
            "id": "4", // Поле типа "Связанный каталог", которое ссылается на каталог «Сотрудники»
            "prevId": "4",
            "duplicateResultWithPrevId": false,
            "name": "Сотрудник",
            "required": false,
            "type": "object",
            "hint": "",
            "isSystem": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "accessOnly": false,
                "enableCreate": true,
                "enableUnsaved": false,
                "enableSelect": true,
                "mode": "cards",
                "defaultEmptyValue": [],
                "type": "object",
                "catalogs": [
                    {
                        "dbId": 3,
                        "id": "$users", // Текстовый id каталога
                        "title": "Сотрудники",
                        "icon": "users-1",
                        "removed": false
                    }
                ],
                "views": [],
                "fields": {
                    "$users": [ // Текстовый ID каталога
                        {
                            "dbId": 1,
                            "id": "$name", // Текстовый ID поля
                            "prevId": "1",
                            "name": "Имя",
                            "columnName": "name",
                            "required": false,
                            "type": "text",
                            "hint": "",
                            "position": 1,
                            "isSystem": true,
                            "history": true,
                            "filterable": true,
                            "apiOnly": false,
                            "duplicateResultWithPrevId": true,
                            "comment": "",
                            "config": {
                                "type": "text",
                                "mask": ""
                            },
                            "visible": {},
                            "createdAt": "2023-11-13T07:57:03.782Z",
                            "updatedAt": "2023-11-13T07:57:03.782Z",
                            "_catalogId": "$users"
                        },
                        {
                            "dbId": 2,
                            "id": "$email", // Текстовый id поля
                            "prevId": "2",
                            "name": "Эл. почта",
                            "columnName": "email",
                            "required": false,
                            "type": "text",
                            "hint": "На указанный адрес будет отправлено приглашение на вход в систему",
                            "position": 2,
                            "isSystem": true,
                            "history": true,
                            "filterable": true,
                            "apiOnly": false,
                            "duplicateResultWithPrevId": true,
                            "comment": "",
                            "config": {
                                "type": "mail",
                                "mask": ""
                            },
                            "visible": {},
                            "createdAt": "2023-11-13T07:57:03.782Z",
                            "updatedAt": "2023-11-13T07:57:03.782Z",
                            "_catalogId": "$users"
                        }
                    ]
                }
            },
            "visible": {}
        }
    ],
    "privilegeCode": "admin",
    "fieldPrivilegeCodes": {}
}
```

</details>

***

#### \[Catalogs.Update]

При изменении структуры каталога для поля типа «Связанный каталог» служебного каталога может указываться ID каталога в обоих форматах.

<details>

<summary>Пример: Изменение структуры пользовательского каталога, который имеет связь со служебным каталогом  «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[PATCH]**:** [https://\[ домен \]/api/v1/catalogs/](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)\[ числовой id каталога ]&#x20;

```json
{
    "name": "Название каталога",
    "fields": [
        {
            "id": "4",
            "name": "Служебные каталоги",
            "type": "object",
            "config": {
                "multiselect": false,
                "mode": "cards",
                "type": "object",
                "catalogs": [
                    {
                        "id": "3" // Числовой ID каталога «Сотрудники»
                    },
                    {
                        "id": "7" // Числовой ID каталога «События»
                    }
                ]
            }
        }
    ]
}    
```

***

_Версия Бипиум 2.0_

_URL_ \[PATCH]**:** [https://\[ домен \]/api/v1/catalogs/](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)\[ числовой или текстовый id каталога ]&#x20;

```json
{   
    "fields": [
        {
            "id": "4",
            "name": "Служебные каталоги",
            "type": "object",
            "config": {
                "mode": "cards",
                "type": "object",
                "catalogs": [
                    {
                        "id": "$users" // Числовой ID каталога «Сотрудники»
                    },
                    {
                        "id": "7" // Числовой ID каталога «События»
                    } 
                ],
                "views": [],
                "fields": {}
            },
            "visible": {}
        }
    ]
} 
```

</details>

### 4. Работа со служебными каталогами (разделы «Управление» и «Система»)

{% hint style="info" %}
Если вы делаете запросы к API для работы с ресурсами, в которых фигурируют каталоги разделов «Управление» и «Система» («Сотрудники», «Компании» и др.), то в качестве ID каталога можно передавать как старый числовой ID, так и новый текстовый. Методы API, обрабатывающие данные каталоги, возвращают текстовые ID каталога и раздела.
{% endhint %}

#### \[Catalogs.Find]

При получении списка всех каталогов служебного раздела «Управление» в возвращаемых данных для каждого каталога будут указаны текстовые ID раздела и каталога.

<details>

<summary>Пример: Данные служебного каталога при получении списка каталогов раздела «Управление»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs?](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)sectionId=1&#x20;

```json
[
    {
        "id": "7", // Числовой ID каталога «События»
        "sectionId": "1", // Числовой ID раздела «Управление»
        "icon": "time-11",
        "chat": {
            "newChats": 0
        },
        "name": "События",
        "history": true,
        "fields": [поля каталога в числовом формате]
    },
    {...}, // Остальные каталоги
    {...}
]
```

***

_Версия Бипиум 2.0_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs?](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)sectionId=$settings

```json
[
    {
        "id": "$events", // Текстовый ID каталога «События»
        "sectionId": "$settings", // Текстовый ID раздела «Управление»
        "icon": "time-11",
        "chat": {
            "newChats": 0
        },
        "name": "События",
        "history": true,
        "fields": [задублированные поля каталога в числовом и текстовом форматах]
    },
    {...}, // Остальные каталоги
    {...}   
]
```

</details>

***

#### \[Catalogs.Get и Catalogs.Update]

При получении/изменении структуры служебного каталога в возвращаемых данных будет указан текстовый ID каталога, а также поля в текстовом и числовом форматах.

<details>

<summary>Пример: Получение структуры каталога «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)/3&#x20;

```json
{
    "id": "6", // Числовой ID каталога
    "sectionId": "1", // Числовой ID раздела
    "icon": "content-34",
    "name": "Сценарии",
    "chat": {
        "newChats": 0
    },
    "privilegeCode": "admin",
    "fieldPrivilegeCodes": {},
    "fields": [ // Поля в числовом формате
        {
            "id": "1",
            "name": "Название",
            "type": "text",
            "hint": "",
            "required": true,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "type": "text",
                "mask": ""
            },
            "visible": null
        },
        {
            "id": "2",
            "name": "Описание",
            "type": "text",
            "hint": "",
            "required": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "type": "multiline",
                "mask": ""
            },
            "visible": null
        },
        {
            "id": "3",
            "name": "Сценарий",
            "type": "file",
            "hint": "",
            "required": false,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "mimeType": "application/bpmn+xml",
                "defaultEmptyValue": []
            },
            "visible": null
        }
    ]
}
```

***

_Версия Бипиум 2.0_

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)/3

_URL_ \[GET]**:** [https://\[ домен \]/api/v1/catalogs](obnovlenie-do-versii-2.0.md#neobratimye-izmeneniya-v-novoi-versii)/$users&#x20;

```json
{
    "id": "$scripts", // Текстовый ID каталога
    "sectionId": "$settings", // Текстовый ID раздела
    "icon": "content-34",
    "chat": {
        "newChats": 0
    },
    "name": "Сценарии",
    "history": true,
    "fields": [ // Поля каталога в числовом и текстовом форматах
        {
            "id": "1",
            "prevId": "1",
            "duplicateResultWithPrevId": true,
            "name": "Название",
            "required": true,
            "type": "text",
            "hint": "",
            "isSystem": true,
            "isDuplicated": true,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "type": "text",
                "mask": ""
            },
            "visible": {}
        },
        {
            "id": "$name",
            "prevId": "1",
            "duplicateResultWithPrevId": true,
            "name": "Название",
            "required": true,
            "type": "text",
            "hint": "",
            "isSystem": true,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "type": "text",
                "mask": ""
            },
            "visible": {}
        },
        {
            "id": "2",
            "prevId": "2",
            "duplicateResultWithPrevId": true,
            "name": "Описание",
            "required": false,
            "type": "text",
            "hint": "",
            "isSystem": true,
            "isDuplicated": true,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "type": "multiline",
                "mask": ""
            },
            "visible": {}
        },
        {
            "id": "$description",
            "prevId": "2",
            "duplicateResultWithPrevId": true,
            "name": "Описание",
            "required": false,
            "type": "text",
            "hint": "",
            "isSystem": true,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "type": "multiline",
                "mask": ""
            },
            "visible": {}
        },
        {
            "id": "3",
            "prevId": "3",
            "duplicateResultWithPrevId": true,
            "name": "Сценарий",
            "required": false,
            "type": "file",
            "hint": "",
            "isSystem": true,
            "isDuplicated": true,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "mimeType": "application/bpmn+xml",
                "defaultEmptyValue": []
            },
            "visible": {}
        },
        {
            "id": "$file",
            "prevId": "3",
            "duplicateResultWithPrevId": true,
            "name": "Сценарий",
            "required": false,
            "type": "file",
            "hint": "",
            "isSystem": true,
            "history": true,
            "filterable": true,
            "apiOnly": false,
            "comment": "",
            "config": {
                "multiselect": false,
                "mimeType": "application/bpmn+xml",
                "defaultEmptyValue": []
            },
            "visible": {}
        }
    ],
    "privilegeCode": "admin",
    "fieldPrivilegeCodes": {}
}
```

</details>

***

**\[Catalogs.Create]**

При создании каталога в служебном разделе в теле запроса должен указываться текстовый ID раздела, при этом на этапе формирования запроса есть возможность задать пользовательский ID создаваемого каталога. В возвращаемых данных будет указан текстовый ID раздела.

<details>

<summary>Пример: Создание каталога со связью на каталог «Сотрудники» в служебном разделе</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]: https://\[ домен ]/api/v1/catalogs

```json
{
    "name": "Новый в каталог в служебном отделе",
    "sectionId": "1", // Числовой ID раздела «Управление»
    "fields": [
        {
            "name": "Просто текст",
            "type": "text",
            "config": {
                "type": "text"
            }
        },
        {
            "name": "Связанный объект: Каталоги Сценарии",
            "hint": "",
            "type": "object",
            "config": {
                "multiselect": false,
                "type": "list",
                "catalogs": [
                    {
                        "id": "6" // Числовой ID каталога «Сценарии»
                    }
                ]
            }
        }
    ]
}
```

***

_Версия Бипиум 2.0_

```json
{
    "name": "Новый в каталог в служебном отделе",
    "sectionId": "$settings", // Текстовый ID раздела
    "id": "newCatalog", // Можно задать текстовый ID
    "fields": [
        {
            "name": "Просто текст",
            "type": "text",
            "config": {
                "type": "text"
            }
        },
        {
            "name": "Связанный объект: Каталоги Сценарии",
            "hint": "",
            "type": "object",
            "config": {
                "multiselect": false,
                "type": "list",
                "catalogs": [
                    {
                        "id": "6" // Числовой ID каталога
                    }
                ]
            }
        }
    ]
}
```

```json
{
    "name": "Новый в каталог в служебном отделе",
    "sectionId": "$settings", // Текстовый ID раздела
    "id": "newCatalog", // Можно задать текстовый ID
    "fields": [
        {
            "name": "Просто текст",
            "type": "text",
            "config": {
                "type": "text"
            }
        },
        {
            "name": "Связанный объект: Каталоги Сценарии",
            "hint": "",
            "type": "object",
            "config": {
                "multiselect": false,
                "type": "list",
                "catalogs": [
                    {
                        "id": "$scripts" // Текстовый ID каталога
                    }
                ]
            }
        }
    ]
}
```

</details>

***

#### \[Boards.Get и Boards.Find]

При получении дашборда(-ов) в возвращаемых данных в атрибуте catalogId будет указан текстовый ID служебного каталога.

<details>

<summary>Пример: Получение дашборда каталога «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]: https://\[ домен ]/api/v1/boards?catalogId=3?viewId=\[id дашборда]

```json
[{
    "id": "6",
    "name": null,
    "catalogId": "3", // Числовой ID каталога «Сотрудники»
    "viewId": null,
    "layouts": {
        "xs": { // параметры отображения графика на широком экране
            "14": { // идентификатор графика
                "x": 0,
                "y": 0,
                "w": 2,
                "h": 10
            }
        },
        "xxs": { // параметры отображения графика на узком экране
            "14": {
                "x": 0,
                "y": 0,
                "w": 1,
                "h": 4
            }
        }
    }
}]
```

***

_Версия Бипиум 2.0_

_URL_ \[GET]: https://\[ домен ]/api/v1/boards?catalogId=3?viewId=\[id дашборда]

_URL_ \[GET]: https://\[ домен ]/api/v1/boards?catalogId=$users?viewId=\[id дашборда]

```json
[{
    "id": "6",
    "name": null,
    "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
    "viewId": null,
    "layouts": {
        "xs": { // параметры отображения графика на широком экране
            "14": { // идентификатор графика
                "x": 0,
                "y": 0,
                "w": 2,
                "h": 10
            }
        },
        "xxs": { // параметры отображения графика на узком экране
            "14": {
                "x": 0,
                "y": 0,
                "w": 1,
                "h": 4
            }
        }
    }
}]
```

</details>

***

#### \[Widgets.Get]

При получении графика в возвращаемых данных в атрибуте catalogId будет текстовый catalogId, в атрибуте filters для полей типа “Связанный каталог” служебного каталога catalogId будет иметь текстовый формат.

<details>

<summary>Пример: Получение графика каталога «Сотрудники», который имеет связь со служебным каталогом</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]: https://\[ домен ]/api/v1/boards/\[id дашборда]\[id графика]

```json
{
    "id": "6",
    "catalogId": "3",// Числовой ID каталога «Сотрудники»
    "boardId": "12",
    "name": "",
    "value": {
        "value": "",
        "type": "recordsCount",
        "subType": null
    },
    "valueFn": "sum",
    "axis": {
        "value": "3",
        "type": "field",
        "subType": null
    },
    "split": null,
    "sort": null,
    "sortType": null,
    "recordsType": "all",
    "recordsFilter": {
        "filters": {
            "3": [
                {
                    "catalogId": "7", // Числовой ID каталога «События»
                    "recordTitle": "Событие 1",
                    "recordId": "1"
                }
            ]
        }
    },
    "chartType": "columns",
    "multicolor": true,
    "stacked": true,
    "limit": null,
    "offset": null,
    "splitLimit": null
}
```

***

_Версия Бипиум 2.0_

```json
{
    "id": "6",
    "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
    "boardId": "12",
    "name": "",
    "value": {
        "value": "",
        "type": "recordsCount",
        "subType": null
    },
    "valueFn": "sum",
    "axis": {
        "value": "3",
        "type": "field",
        "subType": null
    },
    "split": null,
    "sort": null,
    "sortType": null,
    "recordsType": "all",
    "recordsFilter": {
        "filters": {
            "3": [
                {
                    "catalogId": "$events", /// Текстовый ID каталога «События»
                    "recordTitle": "Событие 1",
                    "recordId": "1"
                }
            ]
        }
    },
    "chartType": "columns",
    "multicolor": true,
    "stacked": true,
    "limit": null,
    "offset": null,
    "splitLimit": null
}
```

</details>

***

#### \[AvailableRecords]

При получении доступных связей в возвращаемых данных будут указаны текстовые ID служебных каталогов и разделов.

<details>

<summary>Пример: Получение доступных связей</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]: https://\[ домен ]/api/v1/boards/\[id дашборда]\[id графика]

```json
[
    {
        "sectionId": "1", // Числовой ID раздела «Управление»
        "catalogId": "3", // Числовой ID каталога «Сотрудники»
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "13",
        "recordTitle": "Саша",
        "recordValues": {}
    },
    {
        "sectionId": "1",
        "catalogId": "3",
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "14",
        "recordTitle": "Марат",
        "recordValues": {}
    }
]
```

***

_Версия Бипиум 2.0_

```json
[
    {
        "sectionId": "$settings", // Текстовый ID раздела «Управление»
        "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "13",
        "recordTitle": "Саша",
        "recordValues": {}
    },
    {
        "sectionId": "$settings",
        "catalogId": "$users",
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "14",
        "recordTitle": "Марат",
        "recordValues": {}
    }
]
```

</details>

***

**\[AvailableFilterRecords]**

При получении доступных условий фильтров в возвращаемых данных будут указаны текстовые ID служебных каталогов.

<details>

<summary>Пример: Получение доступных связей каталога </summary>

_Версия Бипиум 1.\*_

_URL:_ [https://\[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)catalogs/\[ id каталога ]/fields/\[ id поля ]/availableFilterRecord

```json
[
    {
        "sectionId": "1", // Числовой ID раздела «Управление»
        "catalogId": "3", // Числовой ID каталога «Сотрудники»
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "13",
        "recordTitle": "Саша"
    },
    {
        "sectionId": "1",
        "catalogId": "3",
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "14",
        "recordTitle": "Марат"
  
    }
]
```

***

_Версия Бипиум 2.0_

```json
[
    {
        "sectionId": "$settings", // Текстовый ID раздела «Управление»
        "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "13",
        "recordTitle": "Саша"
    },
    {
        "sectionId": "$settings",
        "catalogId": "$users",
        "catalogTitle": "Сотрудники",
        "catalogIcon": "users-1",
        "recordId": "14",
        "recordTitle": "Марат"
    }
]
```

</details>

***

**\[Rights.Get и Rights.Create]**

При получении/сохранении прав на каталог в полях rightSubject для служебных каталогов будут указаны текстовые ID каталогов.

<details>

<summary>Пример: Получение прав на каталог «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_ \[GET]: [https://\[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)right?sectionId=1\&catalogId=3

```json
[
    {
        "object": {
            "sectionId": "1" // Числовой ID раздела «Управление»
        },
        "rules": [
            {
                "id": "211",
                "rightSubject": {
                    "catalogId": "3", // Числовой ID раздела «Сотрудники»
                    "catalogDbId": 3,
                    "catalogIcon": "users-1",
                    "recordId": "3",
                    "recordDbId": 3,
                    "recordTitle": "Александр",
                    "userAttr": "id",
                    "userAttrTitle": ""
                },
                "fields": {},
                "type": "direct",
                "privilegeCode": "admin"
            }
        ],
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ]
    }
]
```

***

_Версия Бипиум 2.0_

_URL_ \[GET]: [https://\[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)right?sectionId=$settings\&catalogId=3

_URL_ \[GET]: [https://\[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)right?sectionId=$settings\&catalogId=$users

```json
   {
        "object": {
            "sectionId": "$settings" // Текстовый ID раздела «Управление»
        },
        "rules": [
            {
                "id": "211",
                "rightSubject": {
                    "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
                    "catalogDbId": 3,
                    "catalogIcon": "users-1",
                    "recordId": "3",
                    "recordDbId": 3,
                    "recordTitle": "Александр",
                    "userAttr": "id",
                    "userAttrTitle": ""
                },
                "fields": {},
                "type": "direct",
                "privilegeCode": "admin"
            }
        ],
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ]
    }
]
```

</details>

### 5. Работа с разделами «Управление» и «Система»

{% hint style="info" %}
В API-запросах для ресурсов, в которых фигурируют разделы «Управление» и «Система» нужно передавать в качестве ID раздела новый текстовый ID. Методы API, обрабатывающие данные разделы, возвращают для атрибута _sectionId_ текстовое значение.
{% endhint %}

**\[Sections.Get и Sections.Update]**

При получении/изменении разделов по API в URL нужно указывать текстовый ID, обращение по числовому ID недоступно.

<details>

<summary>Пример: Получение раздела «Управление»</summary>

_Версия Бипиум 1.\*_

_URL:_ [https://\[домен\]/api/v1/sections](https://testcorp.bpium.ru/api/v1/sections)/1

```json
{
    "id": "1",  // Числовой ID раздела «Управление»
    "icon": "setting-7",
    "name": "Управление",
    "privileges": [
        "admin",
        "access",
        "delete",
        "export",
        "create",
        "edit",
        "view",
        "search",
        "available"
    ],
    "privilegeCode": "admin",
    "priority": 15
}
```

***

_Версия Бипиум 2.0_

_URL:_ [https://\[домен\]/api/v1/sections](https://testcorp.bpium.ru/api/v1/sections)/$settings

```json
{
    "id": "settings",  // Текстовый ID раздела «Управление»
    "icon": "setting-7",
    "name": "Управление",
    "privileges": [
        "admin",
        "access",
        "delete",
        "export",
        "create",
        "edit",
        "view",
        "search",
        "available"
    ],
    "privilegeCode": "admin",
    "priority": 15
}
```

</details>

***

#### \[Sections.Find]

При получении списка разделов по API в возвращаемых данных атрибут _sectionId_ будет указан в текстовом формате.

<details>

<summary>Пример: Получение разделов системы</summary>

_Версия Бипиум 1.\*_

_URL:_ \[GET]: https://\[ домен ]/api/v1/sections

```json
[
    {
        "id": "10",  
        "icon": "content-3",
        "name": "Тестовый отдел",
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ],
        "privilegeCode": "admin",
        "priority": 14
    },
    {
        "id": "1",  // Числовой ID раздела «Управление»
        "icon": "setting-7",
        "name": "Управление",
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ],
        "privilegeCode": "admin",
        "priority": 15
    }
]
```

***

_Версия Бипиум 2.0_

```json
[
    {
        "id": "10",
        "icon": "content-3",
        "name": "Тестовый отдел",
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ],
        "privilegeCode": "admin",
        "priority": 14
    },
    {
        "id": "$settings",  // Текстовый ID раздела «Управление»
        "icon": "setting-7",
        "name": "Управление",
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ],
        "privilegeCode": "admin",
        "priority": 15
    }
]
```

</details>

***

#### \[Catalogs.Find]

При получении списка каталогов служебного раздела по API, необходимо указывать строковый ID раздела вместо числового. При этом в возвращаемых данных для всех каталогов будет указан текстовый ID раздела.

<details>

<summary>Пример: Получение списка каталогов раздела «Управление»</summary>

_Версия Бипиум 1.\*_

_URL:_ [https://\[домен\]/api/v1/c](https://testcorp.bpium.ru/api/v1/sections)atalogs?sectionId=1

```json
[
    {
        "id": "7",  // Числовой ID каталога «События»
        "sectionId": "1", " // Числовой ID раздела «Управление»
        "icon": "time-11",
        "chat": {
            "newChats": 0
        },
        "name": "События",
        "history": true,
        "fields": [
            {
                "id": "11",
                "prevId": "11",
                "duplicateResultWithPrevId": false,
                "name": "Секция",
                "required": false,
                "type": "group",
                "hint": "",
                "isSystem": false,
                "history": true,
                "filterable": true,
                "apiOnly": false,
                "comment": "",
                "config": {
                    "multiselect": false,
                    "items": [
                        {
                            "name": "Активно",
                            "id": "$on",
                            "color": "C9FDE5",
                            "dbId": 1
                        },
                        {
                            "name": "Выключено",
                            "id": "$off",
                            "color": "DFE0E0",
                            "dbId": 2
                        }
                    ],
                    "defaultValue": true,
                    "defaultEmptyValue": [
                        "$on"
                    ],
                    "tab": true
                },
                "visible": {}
            }        
        ]
    },
    {...}, // Остальные каталоги секции
    {...}
]    
```

***

_Версия Бипиум 2.0_

_URL:_ [https://\[домен\]/api/v1/c](https://testcorp.bpium.ru/api/v1/sections)atalogs?sectionId=$settings

```json
[
    {
        "id": "$events", // Текстовый ID каталога «Сотрудники»
        "sectionId": "$settings",  // Текствоый ID раздела «Управление»
        "icon": "time-11",
        "chat": {
            "newChats": 0
        },
        "name": "События",
        "history": true,
        "fields": [
            {
                "id": "11",
                "prevId": "11",
                "duplicateResultWithPrevId": false,
                "name": "Секция",
                "required": false,
                "type": "group",
                "hint": "",
                "isSystem": false,
                "history": true,
                "filterable": true,
                "apiOnly": false,
                "comment": "",
                "config": {
                    "multiselect": false,
                    "items": [
                        {
                            "name": "Активно",
                            "id": "$on",
                            "color": "C9FDE5",
                            "dbId": 1
                        },
                        {
                            "name": "Выключено",
                            "id": "$off",
                            "color": "DFE0E0",
                            "dbId": 2
                        }
                    ],
                    "defaultValue": true,
                    "defaultEmptyValue": [
                        "$on"
                    ],
                    "tab": true
                },
                "visible": {}
            }        
        ]
    },
    {...}, // Остальные каталоги секции
    {...}
]
```

</details>

***

#### \[Rights.Get и Rights.Create]

При получении/сохранении прав на раздел «Система» API должен указываться текстовый ID вместо числового. При этом в возвращаемых данных в поле object будет указан текстовый sectionId.

<details>

<summary>Пример: Получение прав на каталог «Сотрудники»</summary>

_Версия Бипиум 1.\*_

URL: https://\[[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)rights?sectionId=3

```json
[
    {
        "object": {
            "sectionId": "1" // Числовой ID раздела «Управление»
        },
        "rules": [
            {
                "id": "211",
                "rightSubject": {
                    "catalogId": "3", // Числовой ID каталога «Сотрудники»
                    "catalogDbId": 3,
                    "catalogIcon": "users-1",
                    "recordId": "3",
                    "recordDbId": 3,
                    "recordTitle": "Александр",
                    "userAttr": "id",
                    "userAttrTitle": ""
                },
                "fields": {},
                "type": "direct",
                "privilegeCode": "admin"
            }
        ],
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ]
    }
]
```

***

_Версия Бипиум 2.0_

URL: https://\[[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)rights?sectionId="$users"

URL: https://\[[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)rights?sectionId=3

```json
   {
        "object": {
            "sectionId": "$settings" // Текстовый ID раздела «Управление»
        },
        "rules": [
            {
                "id": "211",
                "rightSubject": {
                    "catalogId": "$users"," // Текстовый ID каталога «Сотрудники»
                    "catalogDbId": 3,
                    "catalogIcon": "users-1",
                    "recordId": "3",
                    "recordDbId": 3,
                    "recordTitle": "Александр",
                    "userAttr": "id",
                    "userAttrTitle": ""
                },
                "fields": {},
                "type": "direct",
                "privilegeCode": "admin"
            }
        ],
        "privileges": [
            "admin",
            "access",
            "delete",
            "export",
            "create",
            "edit",
            "view",
            "search",
            "available"
        ]
    }
]
```

</details>

***

#### \[AvailableFilterRecords]

При получении доступных связей в возвращаемых данных будут указаны текстовые ID раздела.

<details>

<summary>Пример: Получение доступных связей каталога «Сотрудники»</summary>

_Версия Бипиум 1.\*_

_URL_:  https://\[[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)catalogs/3/fields/\[fieldId]/availableFilterRecord

```json
[
    {
        "sectionId": "1", // Числовой ID раздела «Управление»
        "catalogId": "3", // Числовой ID каталога «Сотрудники»
        "catalogTitle": "Сотрудники",
        "catalogIcon": "transfers-52",
        "recordId": "15",
        "recordTitle": "Сотрудники"
    },
    {
        "sectionId": "2",
        "catalogId": "4",
        "catalogTitle": "Каталог",
        "catalogIcon": "transfers-52",
        "recordId": "26",
        "recordTitle": "Клиенты"
    }
]
```

***

_Версия Бипиум 2.0_

_URL_:  https://\[[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)catalogs/$users/fields/\[fieldId]/availableFilterRecord

_URL_:  https://\[[домен\]/api/v1/](https://testcorp.bpium.ru/api/v1/sections)catalogs/3/fields/\[fieldId]/availableFilterRecord

```json
[
    {
        "sectionId": "$settings", // Текстовый ID раздела«Управление»
        "catalogId": "$users", // Текстовый ID каталога «Сотрудники»
        "catalogTitle": "Сотрудники",
        "catalogIcon": "transfers-52",
        "recordId": "15",
        "recordTitle": "Клиенты"
    },
    {
        "sectionId": "$settings",
        "catalogId": "$users",
        "catalogTitle": "Сотрудники",
        "catalogIcon": "transfers-52",
        "recordId": "26",
        "recordTitle": "Клиенты"
    }
]
```

</details>

### 6. Работа с базой данных (только для серверной версии)

{% hint style="info" %}
Атрибуты таблиц базы данных служебных каталогов переименованы на семантически правильные значения. Необходима актуализация SQL-запросов.
{% endhint %}

**\[База данных]**\
При работе с базой данных системы путем отправки SQL-запросов необходимо использовать переименованные значения атрибутов из [таблиц](obnovlenie-do-versii-2.0.md#tablica-mutirovannykh-dannykh).

<details>

<summary>Пример: Формирование SQL-запроса для работы с каталогом «Сотрудники»</summary>

_Версия Бипиум 1.\*_

```sql
SELECT id, name, description
FROM catalog_123_data
WHERE id = 1;
```

***

_Версия Бипиум 2.0_

```sql
SELECT dbId, name, description
FROM catalog_123_data
WHERE dbId = 1;
```

</details>

## Подготовка к обновлению

Новый текстовый формат ID служебных каталогов может повлиять на работу автоматизаций, веб-хуков, а также стать причиной появления ошибок на стороне интеграционных сервисов, поскольку все они не перестроены на работу с текстовым форматом ID служебных каталогов.

Сущности системы, которые необходимо проверить:

* События
* Внешние запросы
* Сценарии
* Веб-хуки

В целях безопасности и во избежание возникновения потенциально возможных ошибок составлен алгоритм подготовки к обновлению, который обработает каталоги системы и сформирует  список опасных автоматизаций. Данный алгоритм не позволяет определить опасные места на стороне интеграционных сервисов, поэтому их необходимо самостоятельно проверить и, при необходимости, перевести на новый текстовый формат ID.

### Алгоритм подготовки к обновлению

#### 1. Анализ каталогов

Необходимо определить каталоги, которые являются служебными или связаны со служебными каталогами.&#x20;

* **Список служебных каталогов.**\
  Определение списка служебных каталогов является первостепенным, поскольку на основе него будут определяться каталоги, связанные со служебными. По-умолчанию каталоги служебных разделов находятся в разделах «Управление» и «Система», за исключением тех случаев, когда они были собственноручно перенесены в другие разделы. Со списком всех служебных каталогов можете ознакомиться в [примечании](obnovlenie-do-versii-2.0.md#primechanie).<br>
* **Список пользовательских каталогов, которые имеют прямую связь на служебные каталоги.**\
  Определение списка пользовательских каталогов, которые имеют поля типа «Связанный каталог» со ссылкой на служебные каталоги.<br>
* **Список пользовательских каталоги, которые имеют косвенную связь на служебные каталоги (каталоги из п. 2).**\
  Пользовательские каталоги из предыдущего пункта имеют прямую связь на служебные каталоги, однако необходимо также определить список каталогов, которые косвенно связаны со служебными каталогами с той целью, что посредством них возможно получение данных служебных каталогов через цепочку связей.<br>
* **Объединение списков.**\
  Списки, полученные на данном этапе имеют один смысл - они так или иначе связаны со служебными каталогами, поэтому из них можно сформировать общий список. Каталоги данного списка  - _опасные_, поскольку являются или связаны со служебными каталогами.

#### 2.  Анализ событий

По аналогии с каталогами, события в системе делятся на два типа - опасные и безопасные. Опасные события - это события, которые отслеживают опасные каталоги. Безопасные события - это события, которые отслеживают безопасные каталоги, не отслеживают опасные.&#x20;

#### 3. Анализ сценариев

*   **Сценарии безопасных событий и внешних запросов**\
    Сценарии безопасных событий не имеют во входных данных данные опасных каталогов, но их необходимо проверить на наличие в них компонентов, которые могут работать с данными таких каталогов.<br>

    **Список компонентов**:

    * «Получить запись»
    * ««Найти записи»
    * «Изменить запись»
    * «Создать запись»
    * «Структура каталога»
    * «Отправить сообщение»
    * «Веб-запрос»
    * «Запуск процесса»
    * «SQL-запроc» (для серверной версии)

    Если такие компоненты отсутствуют, то сценарий считается безопасным и не нуждается в дальнейших проверках.<br>

    **В случае, если найдены компоненты:**&#x20;

    * «Получить запись»
    * «Найти запись»
    * «Создать запись»
    * «Изменить запись»
    * «Отправить сообщение»
    * «Структура каталога»

    необходимо узнать, данные какого каталога используются. \
    \
    Признаки опасного сценария:

    * Если возвращаются данные опасного каталога.&#x20;
    * Если в сценарии есть компонент «Веб-запро&#x441;_»_, и он отправляется к API Бипиум.
    * Если в сценарии есть компонент «_Запуск процесса»_, и он запускает опасный сценарий и/или в него передаются данные опасных каталогов.&#x20;
    * Если вы используете серверную версию, и в сценарии есть компонент «SQL-запрос», который направлен к базе данных системы.\
      <br>
* **Сценарии опасных событий**\
  Сценарии опасных событий по-умолчанию являются опасными, поскольку входные данные таких сценариев имеют данные опасных каталогов.

#### 4.  Автотест

Для автоматизации предыдущих шагов, можете запустить сценарий, который обнаружит в системе опасные каталоги и автоматизации, и вернет результат в виде в excel-документе.

Перейдите в каталог «Сценарии» и создайте новую запись:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXf64w5Qes8QHMSx2nuCfXm-1jaX5VWG0b8Rlnf8ZRxDPyGNipYpmBdfZi7LM5XpZ8PhD-yPlz5NQKUGQkNzRR6gNon5cEBeaIcZrndz91iIBM4mNAUQepv9XKGzSRtaKArlAJU7utdZPV-nZt2gxHQX1ng?key=MQq_fGFpbyw0rA606aAiDze1" alt=""><figcaption></figcaption></figure>

> **Название**: Название сценария
>
> **Сценарий**: [Сценарий для поиска опасных сценариев](https://drive.google.com/file/d/1BO5Zy2sn1nq2gg7LFxqJrgahVXGpcljq/view?usp=drive_link)

Откройте созданный сценарий и в компоненте «credentions» в переменных _login_ и _password_ укажите ваш логин и пароль для входа в систему:

<figure><img src="../../.gitbook/assets/image (95) (1).png" alt=""><figcaption></figcaption></figure>

Перейдите в каталог «Внешние запросы» и создайте новую запись:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXfdkQ5A24183ZesRIS-mBqSHHe6Gs5SB0V7h7fM6ougonNeILHniktswzCNBjrEybrXI1yPQorkCe3wZsLnIu9o4nf1ePv3b8yEeAUBq6_oRJIsj2ifXvXpgH8GDY4DqO30NAqA-tmINJCNtT_vMam5Fa-y?key=MQq_fGFpbyw0rA606aAiDze1" alt=""><figcaption></figcaption></figure>

> **Статус**: Активно
>
> **Название**: Название внешнего запроса
>
> **URL-идентификатор**: Уникальная строка в каталоге «Внешние запросы»
>
> **Выполнить**: Укажите ранее созданную запись сценария

Для запуска процесса откройте новую вкладку в браузере и в адресной строке введите:

```
{ Адрес сервера Bpium }/api/webrequest/{ URL-идентификатор }?async=true
```

Как только произойдет переход по указанному адресу, система определит внешний запрос и запустит соответствующий сценарий. Результат будет сохранен в Excel-документ. Для его получения перейдите в каталог «Внешние запросы» и откройте созданную ранее запись. В поле «Описание» будет находиться ссылка на скачивание документа.

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Результат представлен на двух листах:

Лист «Список сценариев» включает в себя 3 списка опасных сценариев:

* Сценарии безопасных событий и внешних запросов
* Сценарии опасных событий
* Сценарии, которые не удалось определить

Лист «Опасные каталоги» включает в себя 3 списка опасных каталогов:

* Служебные каталоги
* Каталоги, которые напрямую связаны со служебными
* Каталоги, которые косвенно связаны со служебными

{% hint style="info" %}
Важно! Если по результатам выполнения данного скрипта вы обнаружили неточности или скрипт выполняется неправильно, отправьте соответствующий запрос в поддержку Бипиум на электронную почту: [support@bpium.ru](mailto:support@bpium.ru). Наши специалисты обработают ваше обращение и помогут в решении возникших проблем.
{% endhint %}



#### 5. Анализ вебхуков

На основе списка опасных каталогов определите список веб-хуков, которые срабатывают на опасные каталоги. Сторонние сервисы и системы должны быть готовы к тому, что в данных служебных каталогов атрибут _catalogId_ будет иметь текстовый формат.



### Редактирование опасных сценариев

Редактирование опасных сценариев подразумевает определение и замену во всех компонентах числовой ID служебных каталогов на текстовый.

Компоненты, атрибуты которых поддерживают выражения, могут описывать логику, завязанную на числовых ID служебных каталогов. Среди часто используемых компонентов стоит выделить «Назначение переменны&#x445;_»_, «Блок код&#x430;_»_ и «Услови&#x435;_»_, которые описывают большую часть логики автоматизации.

1. **Проверка на работу с данными опасных каталогов**

* **Проверка на работу с данными из объектов&#x20;**_**allValues**_**,&#x20;**_**prevValues**_**,&#x20;**_**values**_**, которые ссылаются на служебные каталоги**\
  Если компоненты сценария, атрибуты которых поддерживают выражения, имеют в полях для выражений логику, которая завязана на числовых ID служебных каталогов из данных объектов _allValues_, _prevValues_, _values_ и атрибута _catalogId_, то в таких выражениях, в дальнейшем, необходимо будет заменить числовые ID служебных каталогов на текстовые.<br>
*   **Проверка на работу с данными служебных каталогов и каталогов, которые имеют связи со служебными**\
    Необходимо найти в сценарии следующие компоненты, которые работают с опасными каталогами:

    * «Получить запись»
    * «Найти записи»
    * «Создать запись»
    * «Изменить запись»
    * «Структура каталога»
    * «Отправить сообщение»
    * «Веб-запрос»

    Если такие компоненты найдены, необходимо проверить все компоненты сценария, которые поддерживают выражения и работают с данными, полученными из перечисленных компонентов.<br>
* **Проверка SQL-запросов (для серверной версии)**\
  Необходимо найти в сценарии компоненты «SQL-запрос» и проверить, какие данные в них используются. При необходимости, обновить названия атрибутов в соответствии с [таблицей](https://docs.google.com/document/d/1QXHX8hkXUQRYIVYvK1C__AAoJFyDrnGHDjgNp2fXOVs/edit?tab=t.0#heading=h.i9hyoo425jiv).

2. **Редактирование сценариев до обновления**

Данный метод предполагает использование переключателя версий для работы сценариев в обоих форматах ID служебных каталогов.

Перейдите в один из разделов системы, например, в «Управление» и создайте в нем новый каталог с уникальным названием:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXcR6vCqxO2OGh0Yqr5G-fXQxuGKzrRq4yKt1-n8D9zsgPd29qwHMpjBCzZ5A8bo_kBvDMTRYrCCTBIP43G-VZAqdzmuZvLadox6oXmOjpfR2kwBFOvFhLR9ENGIqvN_vfm97XjLcz2xBxu2AwRqRhxvunnw?key=MQq_fGFpbyw0rA606aAiDze1" alt=""><figcaption></figcaption></figure>

> isV2 (Переключатель):
>
> **Описание**: Переключатель для определения используемой версии системы
>
> **Настройки**: По умолчанию: выключен.

> Описание (Текст):
>
> **Описание**: Для отображения информации записи
>
> **Настройки**: Многострочный текст



Создайте в этом каталоге единственную запись, заполнив поля следующими данными:

> * **isV2**: Выключен
> * **Описание**: Произвольный текст для информации



Откройте опасный сценарий, который должен уметь работать в обоих форматах ID и добавьте после компонента «Начало процесса», два компонента:

Компонент «Получить запись»:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdbMeSMouADPWHjktwl47tPGx6O9VrJKPlsr3HOYu59UCX-UlCk3QUyVom0ftzaRmdw-E5Ei9j6U0QBUxX21hVY4jhqcgrin1aaNadRF3cP-Ql8ybV9Bv_sVX1dXNnzmrPzlTUyL6EHUIveq-Jn06J2uKY?key=MQq_fGFpbyw0rA606aAiDze1" alt=""><figcaption></figcaption></figure>

> * **Название**: Название компонента
> * **Каталог**: Созданный ранее каталог для определения в сценариях версии системы
> * **ID записи**: ID записи с информацией по используемой версии
> * **Сохранить в**: Название переменной, в которую будет сохранена запись

Компонент «Назначение переменных»:

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd04CYoLUPogN6gsLnGLvAg0DlcqEr9MEA99JpptjliLL0dUHx2VaX4VnWqCtpnQT0U9IVWWALBtiN7jqGvKE7Nres1Sg8PTioYZloHc_6rNR20JsTENU7A09YnUC9PBN3HIrHp4Fm9aU_Ox2r0sQJGQvLR?key=MQq_fGFpbyw0rA606aAiDze1" alt=""><figcaption></figcaption></figure>

В дальнейшем, в компонентах, найденных по пункту 6, используйте логику с определением текущей версии путем использования переменной _isV2_. Пример с использованием переключателя версий: [Пример](https://drive.google.com/file/d/1w6Y0t6Z_4asoFiHbyphS4P0vE2bLJmKe/view?usp=drive_link)



3. **Редактирование сценариев после обновления**

Данный метод предполагает временную приостановку работы системы во избежание возникновения ошибок. В период остановки необходимо переписать все опасные сценарии, адаптировав их к новому текстовому формату ID служебных каталогов.

Для этого потребуется:

1. Определить все компоненты в сценариях, основываясь на пункте 6, в которых используются числовые ID служебных каталогов.
2. Заменить числовые ID на текстовые.

По окончании изменений сценарии и веб-хуки будут успешно функционировать только на новой версии системы.



## Обновление

Если вы используете серверную версию Бипиум, в целях безопасности рекомендуется снять дамп текущей версии базы данных. В случае неудачного обновления вы сможете использовать его в качестве базы данных при откате версии системы.&#x20;

Для обновления вашей системы в облаке обратитесь с запросом в поддержку Бипиум на почту - [support@bpium.ru](mailto:support@bpium.ru). Наши инженеры проконсультируют Вас по всем вопросам, связанными с обновлением конкретно вашей системы и запустят процедуру обновления.<br>

## Действия после обновления

Если вы выбрали метод подготовки к обновлению, подразумевающий переключение используемой версии, то откройте соответствующую запись в системе и установите переключатель в значение _Включено_.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXd55EsQPrEqC4sK_zL96-qcAbhNeiN_4c4tM3QFhG765Yd2HhStA0Ilpf0crdMfiPbP1FMQha2vgtarp_TyeIZRkXXf4-g0QMC2To4zYxy2IeTEq42qlZYTGUDm9dGOon6AEKlvZ2hh7NKZtFw32I_lOyZp?key=MQq_fGFpbyw0rA606aAiDze1" alt=""><figcaption></figcaption></figure>

Проверьте каталог «Процессы» на наличие ошибок в запущенных сценариев. Если такие процессы есть, изучите ошибку и постарайтесь ее исправить.&#x20;

## Откат системы при неудачном обновлении

{% hint style="danger" %}
**Важно**: Откат к предыдущей версии системы невозможен для облачной версии Бипиум, поскольку в облачной инфраструктуре отсутствует возможность восстановления базы данных из бэкапов, сделанных до обновления.&#x20;
{% endhint %}

Если вы используете серверную версию Бипиум, и у вас есть бэкап базы данных, сделанный до обновления, то:

1. Загрузите и разверните предыдущую версию Бипиум по одной из инструкций для установки в [документации](https://docs.bpium.ru/).
2. Восстановите бэкап базы данных.
3. Запустите службы и проверьте работоспособность системы.

{% hint style="info" %}
Если у вас возникнут трудности при откате версии обратитесь в поддержку Бипиум с подробным описанием проблемы.
{% endhint %}



## Примечание

### Служебные разделы

| **Наименование секции** | **sectionId (number)** | **sectionId(string)** |
| ----------------------- | ---------------------- | --------------------- |
| Управление              | 1                      | $settings             |
| Система                 | 2                      | $system               |



### Список служебных каталогов (секция «Управление»)

| **Наименование каталога** | **catalogId (number)** | **catalogId (string)** |
| ------------------------- | ---------------------- | ---------------------- |
| Каталоги                  | 1                      | $catalogs              |
| Секции                    | 2                      | $sections              |
| Сотрудники                | 3                      | $users                 |
| Виды                      | 4                      | $views                 |
| Вебхуки                   | 5                      | $webhooks              |
| Сценарии                  | 6                      | $scripts               |
| События                   | 7                      | $events                |
| Процессы                  | 8                      | $processes             |
| Доступы к сервисам        | 9                      | $authServices          |
| Внешние запросы           | 10                     | $webRequests           |
| Дашборды                  | 11                     | $boards                |
| Графики                   | 12                     | $widgets               |
| Сообщения                 | 13                     | $messages              |
| <p>Права на поля<br></p>  | 14                     | $rightFields           |
| Права                     | 15                     | $rights                |

<br>

### Список серверных каталогов (секция «Система»)

| **Наименование каталога** | **catalogId (number)** | **catalogId (string)** |
| ------------------------- | ---------------------- | ---------------------- |
| Компании                  | 16                     | $companies             |
| Аккаунты                  | 17                     | $accounts              |
| Приглашения               | 18                     | $invites               |
| APP                       | 19                     | $appTags               |
| API                       | 20                     | $apiTags               |
| Группы компаний           | 21                     | $companiesGroups       |
| Шаблоны писем             | 22                     | $emailTemplates        |
| Расширения                | 23                     | $extensions            |
| Активность доменов        | 24                     | $requests              |

<br>

Как можете заметить, ID каталогов начинаются с символа $. В прошлых версиях Бипиума работа с ними осуществлялось по числовым ID.&#x20;

В прошлых обновлениях Бипиума были постепенно добавлены некоторые из указанных каталогов, в связи с чем порядок последовательного формирования числовых ID может быть нарушен и, например, для каталога Сообщения”, catalogId может не соответствовать указанному в таблице.



### Таблица мутированных данных

Каталог с точки зрения базы данных это 5 таблиц:

* catalog\_\[dbId]\_data - таблица для хранения данных каталога
* catalog\_\[dbId]\_fields - таблица для хранения полей каталога
* catalog\_\[dbId]\_contacts - таблица для хранения типа поля “contact”
* catalog\_\[dbId]\_links - таблица для хранения связанных записей каталога
* catalog\_\[dbId]\_stat\_dropdown\_items - таблица для хранения времени переключения статусов для отображения в графиках

**Каталоги**

<table data-header-hidden><thead><tr><th width="352"></th><th></th><th></th></tr></thead><tbody><tr><td>Таблица</td><td>Старое значение атрибута</td><td>Новое значение атрибута</td></tr><tr><td>catalog_[dbId]_data </td><td>id</td><td>dbId</td></tr><tr><td>catalog_[dbId]_fields</td><td>id</td><td>dbId</td></tr><tr><td>catalog_[dbId]_fields</td><td>_newId</td><td>id</td></tr><tr><td>catalog_[dbId]_contacts</td><td>recordId</td><td>recordDbId</td></tr><tr><td>catalog_[dbId]_contacts</td><td>fieldId</td><td>fieldDbId</td></tr><tr><td>catalog_[dbId]_stat_dropdown_items</td><td>recordId</td><td>recordDbId</td></tr><tr><td>catalog_[dbId]_stat_dropdown_items</td><td>fieldId</td><td>fieldDbId</td></tr><tr><td>catalog_[dbId]_stat_dropdown_items</td><td>itemId</td><td>itemDbId</td></tr><tr><td>catalog_[dbId]_links</td><td>fieldId</td><td>fieldDbId</td></tr><tr><td>catalog_[dbId]_links</td><td>recordId</td><td>recordDbId</td></tr><tr><td>catalog_[dbId]_links</td><td>catalogId</td><td>linkCatalogDbId</td></tr><tr><td>catalog_[dbId]_links</td><td>catalogRecordId</td><td>linkRecordDbId</td></tr></tbody></table>



**Отдельные таблицы**

| Таблица         | Старое значение атрибута | Новое значение атрибута |
| --------------- | ------------------------ | ----------------------- |
| catalogs\_data  | sectionId                | sectionDbId             |
| view\_data      | catalogId                | catalogDbId             |
| view\_data      | userId                   | userDbId                |
| files           | catalogId                | catalogDbId             |
| files           | recordId                 | recordDbId              |
| files           | fieldId                  | fieldDbId               |
| titles          | catalogId                | catalogDbId             |
| titles          | recordId                 | recordDbId              |
| titles          | lastMessageId            | lastMessageDbId         |
| history         | userId                   | userDbId                |
| history         | catalogId                | catalogDbId             |
| history         | recordId                 | recordDbId              |
| view\_filters   | viewId                   | viewDbId                |
| history\_values | fieldId                  | fieldDbId               |
| userSettings    | userId                   | userDbId                |

<br>

**Другое**

| Таблица          | Атрибут     | Старое значение атрибута | Новое значение атрибута |
| ---------------- | ----------- | ------------------------ | ----------------------- |
| catalogs\_fields | columnName  | sectionId                | sectionDbId             |
| messages\_fields | id(\_newId) | $catalogId               | $catalogDbId            |
| messages\_fields | id(\_newId) | $recordId                | $recordDbId             |
| view\_fields     | columnName  | userId                   | userDbId                |
| view\_fields     | columnName  | catalogId                | catalogDbId             |
