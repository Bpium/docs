---
description: >-
  Документ описывает механизмы регистрации событий информационной безопасности,
  включая журналирование действий пользователей и административных операций.
icon: memo-circle-info
---

# Механизмы регистрации и перечень журналируемых событий

## Механизмы регистрации событий

Регистрация событий информационной безопасности осуществляется средствами журналирования службы.\
События фиксируются в системном журнале и доступны для просмотра с использованием стандартных инструментов операционной системы (например, `journalctl`), а также могут быть [перенаправлены в отдельные лог-файлы](logs-integration.md#nastroika-logirovaniya-bipiuma-v-fail).

Данные журналы используются для мониторинга, аудита и расследования инцидентов информационной безопасности.

Журналы событий формируются службой в текстовом формате и содержат временную метку (GMT), источник события (модуль системы), тип операции и дополнительные атрибуты (пользователь, идентификаторы объектов, параметры запроса).

Для корреляции событий используется уникальный идентификатор запроса (marker), позволяющий отследить полный жизненный цикл операции.

## Перечень журналируемых событий

### События аутентификации и авторизации

* успешный вход:

```
Tue, 21 Apr 2026 13:00:12 GMT Router:Login user authenticated, email: admin
Tue, 21 Apr 2026 13:00:12 GMT Auth:Login:Post User "admin" logged in
```

* неуспешный вход:

```
Thu, 23 Apr 2026 08:05:16 GMT Auth:Login:Post User "admin" logged in failed with message:  { message: 'Incorrect password' }
```

* выход из системы:

```
Tue, 21 Apr 2026 12:59:58 GMT Auth:Logout User "1" logged out
```

### Управление пользователями и доступом

* создание пользователя:

```
Wed, 08 Apr 2026 12:39:22 GMT Router:Auth:Register:Post Register account begin: user@example.com
Wed, 08 Apr 2026 12:39:22 GMT Register: Account create begin:  user@example.com
```

* изменение прав:

```
Wed, 08 Apr 2026 12:44:05 GMT Api:Request 50:Rights:Create: by object "{ catalogId: '25' }"
Wed, 08 Apr 2026 12:44:05 GMT Api:Request 50:Record:Create:Catalog:$rights: params {
  values: {
    '$object': [ [Object] ],
    '$subjectAttr': 6,
    '$subject': [ [Object] ],
    '$privileges': [ '$view', '$search', '$available' ],
    '$fieldsPrivileges': []
  }
}
```

* изменение пароля пользователя:

```
Tue, 21 Apr 2026 12:58:09 GMT EventHandler:Record:AfterUpdate update user password, email: user@example.com
```

* удаление пользователя:

```
Tue, 21 Apr 2026 13:00:43 GMT Router:Api request marker=210 started for  DELETE domain.bpium.ru/api/v1/catalogs/$users/records/1?timezoneOffset=180&skipPrevId=true
Tue, 21 Apr 2026 13:00:43 GMT Router:Api User "admin" request marker=210
```

* назначение прав доступа:

```
Wed, 08 Apr 2026 12:44:05 GMT API:Router:Log POST /v1/rights
Wed, 08 Apr 2026 12:44:05 GMT Api:Request 50:Rights:Create
```

### Административные действия

* запуск сервиса:

```
Wed, 08 Apr 2026 12:36:37 GMT Router: Router init
Wed, 08 Apr 2026 12:36:37 GMT Server:StartHttp Start listen port to https 81
Wed, 08 Apr 2026 12:36:37 GMT Server:StartHttp Start listen port to http 80
```

