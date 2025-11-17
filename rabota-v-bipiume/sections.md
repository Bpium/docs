# Интерфейс системы

Отделы служат для наведения порядка. С помощью них вы можете группировать каталоги со связанными по смыслу данными. В Бипиуме нет ограничения на число отделов.

## Список разделов

<figure><img src="../.gitbook/assets/1. Список отделов.png" alt=""><figcaption></figcaption></figure>

Разделы отображены в верхней части экрана. По умолчанию в Бипиуме создан один отдел — «Управление».

## Создание нового раздела

В конце списка разделов находится кнопка добавления новых отделов. Отделы могут создавать сотрудники имеющие право администрировать отдел «Управление».

<figure><img src="../.gitbook/assets/1. Создание отдела.png" alt=""><figcaption></figcaption></figure>

Созданные разделы видны всем сотрудникам компании, пока на них не ограничили права.

## Изменение раздела

В нижней-левой части экрана расположена кнопка управления "Свойства раздела":

* Доступ к разделу — назначение прав на раздел
* Переименовать — изменение названия
* Удалить раздел — удаление раздела со всеми каталогами и данными

<figure><img src="../.gitbook/assets/1. Изменение отдела.png" alt=""><figcaption></figcaption></figure>

В зависимости от назначенных прав набор действий может быть ограничен.

## Удаление раздела

При удалении отдела также удаляются и все каталоги этого отдела. При этом, если другие записи ссылаются на записи удаленных каталогов, то эти связи останутся. Но открыть удаленные связанные записи будет нельзя.

### Восстановление удаленных данных

Удаленный раздел и каталоги становятся недоступны через приложение или API. Однако они сохраняются в базе данных, для возможности восстановления в случае ошибочного удаления. Для консультации по восстановлению данных напишите запрос на [support@bpium.ru](mailto:support@bpium.ru).

## Каталоги

Каталог — набор однотипных записей. Например, список клиентов, заказов, обращений, городов.

## Список каталогов

Все каталоги в Бипиуме находятся внутри отделов и показаны списком слева:

![](<../.gitbook/assets/1 - Список каталогов 2.png>)

Экран каталога состоит из двух частей:

* Левая панель для фильтрации записей
* Основная часть с отображением записей

## Создание нового каталога

В конце списка каталогов отдела находится кнопка создания нового каталога. Кнопка будет скрыта, если у сотрудника нет прав на администрирование отдела.

При создании нового каталога вы переходите в режим [редактирования его структуры](broken-reference).

## Изменение каталога

В верхней части левой панели расположена кнопка управления свойствами каталога:

* Доступ к каталогу — назначение прав на каталог и его записи
* Настроить поля — изменение названия, иконки и структуры данных
* Удалить все записи — удаление всех записей в каталоге
* Удалить каталог — удаление каталога со всеми данными

![](<../.gitbook/assets/3 - Изменение каталога 2 (1).png>)

В зависимости от назначенных прав набор действий может быть ограничен.

**Удаление всех записей**

При удалении всех записей каталога также происходит удаление всех связей и историй каждой из удаляемых записей. Для подтверждения удаления всех записей система попросит повторно ввести название каталога во избежание случайной очистки. Механизм удаления всех записей в каталоге оказывается полезен, когда необходимо быстро очистить все содержимое каталога, не прибегая к сценариям автоматизаций. Такое, например, возникает при разработке каталога и наполнения его тестовыми записями, которые в последствие не будут использоваться.

## Сортировка каталогов

Каталоги можно разместить в нужном порядке, перетащив их мышкой за иконку каталога. Сортировать каталоги может сотрудник с правом администрировать отдел.



### Перенос каталога между отделами <a href="#perenos-kataloga-mezhdu-otdelami" id="perenos-kataloga-mezhdu-otdelami"></a>

В настройках структуры каталога можно перенести каталог между отделами. В верхней части левой панели необходимо выбрать нужный отдел из списка всех отделов. Перемещение каталога происходит без потери данных.

<figure><img src="https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-M28GbnLoddrTSSCyErs%252F-M28wAnx5mUeUjTBXksv%252F1.png%3Falt%3Dmedia%26token%3D391c1f13-981d-483b-9033-cfe0e5aba8d1&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=83b1ee8a&#x26;sv=2" alt=""><figcaption></figcaption></figure>

### Название каталога и иконка <a href="#nazvanie-kataloga-i-ikonka" id="nazvanie-kataloga-i-ikonka"></a>

Каталог имеет иконку и название. Чтобы изменить иконку, нажмите на кнопку около названия каталога.

<figure><img src="https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-LtOSgxj2dIcH4vILl3d%252F-LtOiyjiOlOnIJwc-7X6%252F1-screenshot.png%3Falt%3Dmedia%26token%3D7f813dbc-c7ba-48ec-94b9-3ca22281902f&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=13561b34&#x26;sv=2" alt=""><figcaption></figcaption></figure>

Бипиум предлагает на выбор более 1000 иконок. Иконки разделены по категориям для удобства навигации.

### Настройка полей <a href="#nastroika-polei" id="nastroika-polei"></a>

<figure><img src="https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-M4nLNelr7LdUFvriuJT%252F-M4nTBqI7t6peXzcSY9M%252Fimage.png%3Falt%3Dmedia%26token%3D1d34c3ae-63cf-4087-8dd4-f2e48ffc1809&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=a7aaad3a&#x26;sv=2" alt=""><figcaption></figcaption></figure>

Структура каталога состоит из полей разного типа. В левой части расположен список возможных элементов (типов полей). В правой — карточка каталога. Поля в карточке разбиты на секции. Секции объединяют близкие по смыслу поля для большего порядка.

Чтобы добавить в каталог новое поле, мышкой перетащите его из левой панели в свободное место анкеты справа.

Поля в карточке можно менять местами перетаскиванием, схватив за иконку поля.

Каждое поле имеет:

* заголовок
* признак обязательного поля (![](https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-LACZnWTX_UP3XcilXPc%252F-LACZvFsUnNloj9qLjcj%252Frequired_mark.jpg%3Fgeneration%3D1523867434483430%26alt%3Dmedia\&width=300\&dpr=4\&quality=100\&sign=3b878dce\&sv=2))
* свойства в зависимости от типа поля
* подсказку, которая отображается сотрудникам при заполнении поля
* свойство, которое разрешает редактирование только через API

![](https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-LtOkWfXCK1D6MTwgiK7%252F-LtOvY2y1ib9YnqvuHSZ%252F3-screenshot.png%3Falt%3Dmedia%26token%3D02aaa6a6-0dc2-462a-884b-36b2134d4da0\&width=768\&dpr=4\&quality=100\&sign=d76f03ba\&sv=2)![](https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-M4nLNelr7LdUFvriuJT%252F-M4nRUTn1869njDzM4T-%252Fimage.png%3Falt%3Dmedia%26token%3D102943f2-64f1-42ce-8f11-b2f5cc76532e\&width=768\&dpr=4\&quality=100\&sign=cdf113be\&sv=2)

### Условия видимости поля <a href="#usloviya-vidimosti-polya" id="usloviya-vidimosti-polya"></a>

С помощью данной настройки вы можете скрыть поле, пока в другом поле не будет указано нужное значение.

В данном примере, поле будет скрыто пока пользователь не укажет "Статус1".

<figure><img src="https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-M4nLNelr7LdUFvriuJT%252F-M4nVm71i10hnLU002Tt%252FTEIFrvXn5Z.gif%3Falt%3Dmedia%26token%3Dcedadbb4-28ae-4877-b357-52e3881abbdf&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=c3597f48&#x26;sv=2" alt=""><figcaption></figcaption></figure>

Настройка производится в панели свойств поля. Для того, чтобы открыть панель свойств необходимо выделить нужное поле:

<figure><img src="https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-M4rsh7xxduqwOuMOXMy%252F-M4ruOFq2fGDJLXzFORW%252F%25D0%25B8%25D0%25B7%25D0%25BE%25D0%25B1%25D1%2580%25D0%25B0%25D0%25B6%25D0%25B5%25D0%25BD%25D0%25B8%25D0%25B5.png%3Falt%3Dmedia%26token%3D13756da1-414b-4e0f-b6ab-7bb1aec45f05&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=18588877&#x26;sv=2" alt=""><figcaption></figcaption></figure>

Далее в панели свойств поля откройте секцию "Видимость" и выберите значения других полей, при которых открытое на редактирование поле должно быть видимым:

<figure><img src="https://docs.bpium.ru/~gitbook/image?url=https%3A%2F%2F1283378397-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LACZmmM2xUWbZxyRr4s%252F-M4rsh7xxduqwOuMOXMy%252F-M4ruI38sSJ0fpdD3m1w%252F%25D0%25B8%25D0%25B7%25D0%25BE%25D0%25B1%25D1%2580%25D0%25B0%25D0%25B6%25D0%25B5%25D0%25BD%25D0%25B8%25D0%25B5.png%3Falt%3Dmedia%26token%3D7c887de7-5638-484f-9e82-dd0c4d7bfbcf&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=22c52d7c&#x26;sv=2" alt=""><figcaption></figcaption></figure>
