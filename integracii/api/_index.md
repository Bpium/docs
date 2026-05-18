---
title: API
order: 0.8
---

API Bpium построено на идеологии [REST](https://ru.ruwiki.ru/wiki/REST?ysclid=mnpyfdya5a85709849), формат передачи данных -- JSON.

## Ресурсы

API состоит из ресурсов.

### Работа с пользовательскими и системными данными

-  [Каталоги (Catalog)](./data/catalogs) -- список каталогов с описанием структуры полей

-  [Записи (Records)](./data/records) -- список записей каталога со значениями полей

-  [Связи (Relations)](./data/relations-relations) -- список записей, которые ссылаются на указанную запись

-  [История (Histories)](./data/istoriya-history) -- список операций по изменению записи

-  [Файл (Files)](./data/files) -- информация о файле в файловом хранилище

-  [Отделы (Sections)](./data/sections) -- список отделов (групп каталогов)

-  [Виды (Views)](./data/views) -- список видов каталога с условиями фильтрации записей

### Агрегация / сводные данные

-  [Разложение (Values)](./agregate/values) -- разложение записей каталога по полю с подсчётом сводных значений

-  [Сводка (Totals)](./agregate/totals) -- подсчёт сводного значения в каталоге по полю

### Отчёты

-  [Дашборды (Boards)](./reports/boards) -- список экранов с графиками и параметрами

-  [График (Widgets)](https://docs.bpium.ru/reports/widgets) -- значения графика

### Дополнительные поисковые выборки

-  [Доступные связи (AvailableRecords)](./search/availablerecords) -- список записей, доступных для связывания в поле типа «связанный объект»

-  [Сотрудники (Users)](./search/polzovateli-users) -- список сотрудников, доступных в системе

-  [Доступные условия фильтра (AvailableFilterRecords)](./search/availablerecords-1) -- список записей для условий фильтрации

-  [Контакты (Contacts)](./search/polzovateli-users-1) -- список записей с поиском по полям контактов

### Правовая политика

-  [Права (Rights)](./prava-rights) -- список правил доступа к объектам

### Связанные с событиями и процессами

-  Лайв-события (Changes) -- вызов события «изменено поле во время редактирования» для обработки вводимых данных

### Системные

-  [Профиль (Profile/me)](./search/polzovateli-users) -- информация о текущем пользователе

-  Компания (Company) -- информация о текущей компании

-  Компании (Companies) -- список компаний пользователя

-  Лицензия (License) -- информация о лицензии

## Методы

Ресурсы имеют CRUD-методы:

-  **Create** -- создание

-  **Read** -- получение

-  **Update** -- изменение

-  **Delete** -- удаление

### HTTP-методы

-  Получить коллекцию ресурсов -- `GET`

-  Получить ресурс -- `GET`

-  Создать ресурс -- `POST`

-  Изменить ресурс -- `PATCH`

-  Удалить ресурс -- `DELETE`

## Формат запросов

API Бипиума использует формат передачи данных `JSON`, кодировка `utf-8`.

HTTP-заголовок:

```http
content-type: application/json
```

## Доступ к API

### Относительный адрес API

```text
/api/v1/{ресурс}/{?ID-объекта}
```

### Адрес API в облаке

```text
https://{вашдомен}.bpium.ru/api/v1/{ресурс}/{?ID-объекта}
```

## Авторизация

Запросы к API проходят проверку прав доступа. Все запросы выполняются от имени сотрудника, права которого используются при обработке запросов.

### Базовая авторизация

Параметры передаются через HTTP-заголовок `Authorization`.

Пример:

```http
Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
```

Подробнее о стандарте: [Basic authentication](https://bewave.ru/blog/nastroyka-http-basic-auth-v-trekh-slovakh/?ysclid=mnpzn6jnt8283763641)

:::info 

В ответ на запрос с базовой авторизацией сервер вернет сессионную cookie `sid` или cookie с другим именем, указанным в конфигурации сервера.

Для облака `bpium.ru` используется cookie `connect.sid`.

Рекомендуется использовать эту cookie в последующих запросах -- это ускоряет работу API.

:::

### Авторизация через POST

Можно отправить POST-запрос на адрес:

```text
https://{вашдомен}.bpium.ru/auth/login
```

Параметры:

-  `email`

-  `password`

:::info 

После авторизации также рекомендуется использовать сессионную cookie.

:::

### Авторизация через Cookie

Пример HTTP-заголовка:

```http
cookie: connect.sid=значение; другиеКуки=...
```

## Часовой пояс

Сервер Bpium хранит даты в UTC.

Для корректной обработки дат клиент должен передавать параметр `timezoneOffset` в минутах.

Пример для UTC+3:

```text
https://{вашдомен}.bpium.ru/api/v1/catalogs/5?timezoneOffset=180
```

## Коды HTTP-ответов

| Код   | Описание                      |
|-------|-------------------------------|
| `400` | Некорректный запрос           |
| `401` | Ошибка авторизации            |
| `402` | Требуется оплата лицензии     |
| `403` | Доступ запрещён               |
| `404` | Ресурс не найден              |
| `405` | Метод не разрешён             |
| `426` | Требуется расширение лицензии |
| `429` | Превышен лимит запросов       |
| `500` | Внутренняя ошибка сервера     |
| `501` | Метод не реализован           |

## Лимиты / Ограничения

Бипиум ограничивает количество запросов к API.

Для облака `bpium.ru` действуют лимиты:

-  `100` запросов за `30` секунд для одной учётной записи

-  `500` запросов за `30` секунд для одного IP-адреса

При превышении лимита сервер возвращает ошибку:

```text
429 Too Many Requests
```

### Параметры лимитов в Enterprise

-  `LIMIT_USER_MAX_REQUESTS` -- лимит запросов пользователя

-  `LIMIT_USER_REQUEST_DURATION` -- интервал проверки пользователя

-  `LIMIT_IP_MAX_REQUESTS` -- лимит запросов IP

-  `LIMIT_IP_REQUEST_DURATION` -- интервал проверки IP

## Примеры использования

### JavaScript (Node.js и frontend)

Публичная библиотека:

```text
https://www.npmjs.com/package/bp-api
```

### PHP

Пример создания записи:

```php
// домен и авторизация
$domen = 'ВАШДОМЕН.bpium.ru';
$user = 'abc@mail.ru';
$pass = 'mypass';

// номер каталога
$catalog_id = 13;

// значения полей
$values = array();
$values['2'] = '';
$values['3'] = array('1');
$values['4'] = 17;
$values['5'] = date('Y-m-d') . "T" . date('H:i:s') . "+04:00";

// тело запроса
$data = array();
$data['values'] = $values;
$data_json = json_encode($data);

// HTTP-запрос
$ch = curl_init("https://$domen/api/v1/catalogs/$catalog_id/records");
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_json);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
curl_setopt($ch, CURLOPT_USERPWD, "$user:$pass");
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_json))
);

$result = curl_exec($ch);
```