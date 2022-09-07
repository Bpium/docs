# Отправка почты

Используется для отправки email-сообщений по протоколу SMTP.

## Свойства

### Секция «Подключение»

**Способ подключения**  \
****Позволяет выбрать способ задания параметров подключения к почтовому серверу. Возможные варианты: `параметры` и `строка подключения`. Вариант `параметры` позволяют указать параметры подключения по отдельности. Вариант `строка подключения` — единой строкой со всеми параметрами.

_Способ подключения: Параметры_

**Адрес сервера**  \
****Адрес (домен или IP-адрес) почтового сервера без указания протокола. Например, "smtp.yandex.ru". Формат: **** «значение в кавычках» или выражение.

**Порт**  \
****Порт почтового сервера. Например, 465. Формат: **** «значение в кавычках» или выражение.

**Шифрование**  \
****Указывает использовать ли защищенное соединение (SSL) для подключения к почтовому серверу. Вариант «не использовать» сначала пробует соединиться используя технологию STARTTLS, при неуспехе пробует без шифрования.

**Логин**  \
****Логин для авторизации на почтовом сервере. Как правило совпадает с адресом электронной почты. Формат: **** «значение в кавычках» или выражение.

**Пароль**  \
****Пароль для авторизации на почтовом сервере. Как правило совпадает с паролем от адреса электронной почты. Формат: **** «значение в кавычках» или выражение.

_Способ подключения: Строка подключения_

**Строка подключения**\
****Строка подключения к почтовому серверу со всеми параметрами.\
Формат: [https://nodemailer.com/smtp/](https://nodemailer.com/smtp/)

### Секция «Письмо»

**От**  \
****Адрес отправителя письма. Допустимо указывать адрес электронный почты или строку с именем отправителя конструкцией вида: «Имя \<email@email.ru>». Формат: **** «значение в кавычках» или выражение. Примеры:

```javascript
"bpium@bpium.ru"
"Bpium <bpium@bpium.ru>"
```

**Кому**  \
****Адрес электронной почты получателя отправителя письма. Адресатов может быть несколько. Формат: список «значений в кавычках» или выражений. Примеры:

```javascript
"user1@bpium.ru"
"user1@bpium.ru, user2@bpium.ru, user3@bpium.ru"
```

**Тема**  \
****Заголовок письма. Формат: **** «значение в кавычках» или выражение.

**Текст**  \
****Содержание письма. Формат: **** «значение в кавычках» или выражение.

Для удобства ввода содержания писем используйте выражение а формате шаблонов. Шаблоны позволяют использовать многострочный текст и переменные внутри текста. В отличие от текстовых констант шаблоны заключаются в обратные одинарные кавычки (\`), а не обычные двойные (") или обычные одинарные ('). Подробнее в статье «[Выражения](../expressions.md)». Пример использования шаблона в качестве значения выражения:

```javascript
`Здравствуйте, ${name}!
Рады сообщить вам, что...`
```

**Формат**  \
****Определяет формат содержания письма: `Простой текст` или `HTML`. В случае `простого текста` письмо будет отправлено без форматирования, в случае `HTML` — в формате HTML, это значит что в тексте письма могут быть использованы разрешенные для писем HTML-теги.

**Вложения**  \
****Вложения к письмо. Бипиум поддерживает прикрепление файлов, размещенных где-либо в интернете. Например файлов прикрепленных к записям в каталогах и хранящимся в облачном хранилище. Вложений может быть несколько. Чтобы прикрепить к письму файл укажите в это свойство объект с 2 параметрами:

```javascript
{
    title: 'Название файла.doc',
    url: 'http://URL к файлу'
}
```

Если требуется прикрепить несколько файлов — укажите массив из таких объектов:

```javascript
[
    {
        title: 'Договор.doc',
        url: 'http://URL к файлу'
    },
    {
        title: 'Счет.xls',
        url: 'http://URL к файлу'
    }
]
```

## Пример прикрепления файлов из поля записи каталога

Файлы в записях каталога также хранятся в массиве и могут использоваться без преобразований.\
Если к записи прикреплен один файл, то достаточно просто передать данные из поля «файл»:

```javascript
[
    {
        id: 1,
        title: "Документ.pdf",
        size: 1024,
        url: "https://...",
        mimeType: "application/pdf",
        metadata: null
    }
]
```

Вышеуказанное представляет собой коллекцию данных из поля «Файл», однако система возьмет только значения по ключам «title» и «url».

## Параметры подключения к популярным сервисам

### Яндекс

* Адрес сервера: **"smtp.yandex.ru"**
* Порт: **465**
* Шифрование: **использовать**
* Логин: **полный адрес эл.почты**
* Пароль: **ваш пароль**

### Gmail

* Адрес сервера: **"smtp.gmail.com"**
* Порт: **465**
* Шифрование: **использовать**
* Логин: **полный адрес эл.почты**
* Пароль: **ваш пароль**

{% hint style="info" %}
**Если письма не отправляются**\
****Gmail может заблокировать отправку письма, так как письма отправляются с наших серверов. Google может посчитать их подозрительными, так как они находятся в непривычном для вас территориальном месте.

Чтобы решить проблему, зайдите на страницу со списком последних устройств, которые пытались авторизоваться — [https://myaccount.google.com/device-activity](https://myaccount.google.com/device-activity).\
Вы увидите попытку входа из Дублина/Ирландии и вопрос, считать ли это подключения вашим. Подтвердите.
{% endhint %}

## Пограничные события

![](../../.gitbook/assets/boundary\_any.png)

Компонент поддерживает 2 типа пограничных событий:

* Ошибка — выход из компонента, если произошла какая-либо ошибка
* Таймаут — выход из компонента, спустя заданное ограничение по времени

Если компонент завершился с ошибкой, но на нем не было пограничного события, то процесс завершается. Сообщение ошибки возвращается в результатах процесса.