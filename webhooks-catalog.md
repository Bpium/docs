# Вебхуки

Когда сотрудники изменяют данные, Бипиум генерирует события. На эти события могут подписаться сторонние системы. Такие подписки называются вебхуки (webhooks).

В Бипиуме вебхуки информируют сторонние системы об изменениях в записях: создании новых, изменении значений полей, удалении записей. Бипиум не информирует сторонние системы об изменении структуры каталогов и правилах правовой политики.

## Создание вебхуков

Список подписок хранится в каталоге Управление / События (webhooks). Каждая запись — отдельная подписка, отслеживающая изменения в определённом каталоге.

![](.gitbook/assets/webhook\_form.jpg)

Подписки имеют следующие параметры:

* Каталог — указывает в каком каталоге отслеживать изменения
* События — список типов событий, которые будут пересылаться в стороннюю систему
* Отслеживать поля — список id полей изменения которых будут отслеживаться
* URL — адрес, на который будет отправлено событие (требуется указывать протокол)
* Секретная строка — ключ, которым будут подписаны сообщения для подтверждения подлинности отправителя
* Номер последнего события — счетчик сработанных событий (служебное поле)

### События

Бипиум поддерживает 3 вида событий: уведомления, запросы и действия.

### Виды событий

**Уведомления** срабатывают ПОСЛЕ выполнения операции.

**Запросы** срабатывают ДО выполнения операции и ожидают подтверждения от сторонней системы на совершение действия. Сторонние системы могут разрешить операцию или отклонить её, вернув текст ошибки сотруднику.

**Действия** срабатывают после изменения поля во время редактирования карточки, до сохранения записи.

### Типы событий

* Уведомление о создании записи (`record.created`) — отправляется после создания записи
* Уведомление об изменении записи (`record.updated`) — отправляется после изменения записи
* Уведомление об удалении записи (`record.deleted`) — отправляется после удаления записи
* Запрос на создание записи (`record.before.created`) — перед созданием, ожидает одобрения
* Запрос на изменение записи (`record.before.updated`) — перед изменением, ожидает одобрения
* Запрос на удаление записи (`record.before.deleted`) — перед удалением, ожидает одобрения
* Изменено поле во время редактирования (`record.updating`) — отправляется после изменения поля во время редактирования записи

Одна подписка может отслеживать и пересылать несколько событий.

## Использование вебхуков

Принцип работы и формат сообщений описан в разделе по интеграции в статье «[Вебхуки](https://github.com/bpium/bpium-documentation/tree/3eee69fa93775fc88bf609ca5696e1f9581d33fa/webhooks.html)».