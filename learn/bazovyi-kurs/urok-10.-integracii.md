---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Урок 10. Интеграции

На предыдущем уроке мы настраивали BPM-сценарий. Сегодня мы расскажем, как в Бипиуме происходит интеграция со сторонними сервисами и какую роль в этом занимают BPM-сценарии.

### Обдумываем

Часто в компаниях одновременно работают в разных системах: фиксирует клиента в CRM, планируют звонок в календаре, напоминают клиенту о предстоящем звонке через почту. Это неудобно и можно что-то забыть. Выход — интегрироваться с календарём и почтой, чтобы информация туда попадала автоматически. В таких кейсах Бипиум может автоматически отправлять данные в любые другие системы.

Бипиум может также и принимать данные из других систем. Сегодня мы покажем, как с помощью BPM-сценариев Бипиум принимает заявки с сайта и автоматически формирует карточку клиента.

***

### Воплощаем

Настроим BPM-сценарий получения заявки с сайта. Сценарий будет запускаться при заполнении формы на сайте. Для этого создадим Событие в Бипиуме, которое будет запускать процесс. Процесс создаст новую «Заявку с сайта». А чтобы в нее подтянулись данные с сайта: ФИО, компанию и телефон, обязательно сообщим разработчику сайта входные параметры — в каком виде данные должны передаваться в Бипиум.

1. **Создадим каталог «Заявки с сайта»**. Добавьте в него три поля: поле Текст назовите — «ФИО», поле Текст — «Компания», поле Контакт — «Телефон». Если у вас есть другие поля — добавляйте.

{% embed url="https://www.youtube.com/watch?ab_channel=Bpium&v=UVVL3XtcROY" %}

2. **Настроим Внешний запрос**. Он будет получать данные с сайта и запускать сценарий на создание записи. Перейдем в отдел «Управление», каталог ВНЕШНИЕ ЗАПРОСЫ и кликаем «Добавить». Заполняем название — «Заявка с сайта», придумываем URL-идентификатор латиницей, у меня это будет «zayavka» и создаем сценарий который будем выполнен при этом запросе. В выпадающем списке поля «Выбрать» выберем «добавить в Сценарии». Присваиваем название сценарию, например «Регистрация заявки» и сохраним. Настроим сценарий чуть позже. Сохраняем сценарий и внешний запрос.\
   \
   ‍_Подробности о Внешнем запросе здесь:_ [_docs.bpium.ru/processes/events/webrequests_](https://docs.bpium.ru/processes/events/webrequests)

{% embed url="https://www.youtube.com/watch?ab_channel=Bpium&v=SFZj7PBkZrY" %}

3. **Определим API полей.** Заходим в настройки каталог ЗАЯВКИ С САЙТА и аналогии с предыдущем уроком, записываем номера полей, данные из которых хотим автоматически заполнить в будущей Заявке. Под каждым полем указан номер API поля. У меня это API: 2, 3, 4.

{% embed url="https://www.youtube.com/watch?ab_channel=Bpium&v=3-61xtkF4mo" %}

4. **Передаем информацию разработчику сайта.** Для того, чтобы заявка поступала с сайта в Бипиум, разработчик сайта должен произвести настройки на стороне сайта. Передадим ему придуманную нами информацию:\
   URL — адрес, на который сайт должен отправлять данные формы, формируется из URL-идентификатора, мы назвали его ZAYAVKA.\
   Получилось: [https://**вашдомен**.bpium.ru/api/webrequest/**zayavka**/](https://xn--80adhe8ahe2f.bpium.ru/api/webrequest/zayavka/)\
   Метод запроса — POST\
   Названия полей формы — пусть это будет fio, company, tel
5. **Настроим Сценарий.** В каталоге СЦЕНАРИИ выбираем только что созданный сценарий — «Регистрация заявки». В поле «Сценарий» кликаем кнопку — «Создать», откроется редактор. Сценарий запускается с входными параметрами:body (объект) — значения полей формы с сайта (POST-параметры).В сценарии их можно получить так: body.fio, body.company, body.telquery, headers и другие — описаны в документации
6. **Создадим Заявку в сценарии.** Из левой части экрана перетащим в основное окно компонент «Создать запись» и установим его между началом и концом процесса. Нажимаем на компонент «Создать запись», в правой части экрана откроется карточка компонента: в поле Каталог выбираем каталог, в котором будет создаваться новые записи — «Заявки с сайта». Ниже заполняем «Значения полей». Каждая строка этого списка состоит из 2 частей: первая часть — номер API поля в карточке «Заявки с сайта», второе — значение, которое должно быть туда записано. Это может быть текст, дата или значение из переменной. У нас это вышло так:\
   \
   2 = body.fio\
   3 = body.company\
   4 = \[ {contact: body.tel} ]\
   \
   ‍_Почему именно такие значения, смотрите в описании компонента:_ [_docs.bpium.ru/processes/scripts/components/createrecord_](http://docs.bpium.ru/processes/scripts/components/createrecord)

{% embed url="https://www.youtube.com/watch?ab_channel=Bpium&v=DOrbu2cOqn8" %}

7.  **Сохраняем сценарий.** После сохранения схемы сценария, не забудьте сохранить саму запись сценария в каталоге. Для запуска нужно будет заполнить форму на сайте, созданную разработчиком.\


    Сегодня мы настроили BPM-сценарий, который создает заявку с сайта.\
    ‍\
    _Подробнее о возможностях интеграции описано здесь:_ [_docs.bpium.ru/integration_](http://docs.bpium.ru/integration)