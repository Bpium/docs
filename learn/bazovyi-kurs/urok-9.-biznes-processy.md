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

# Урок 9. Бизнес-процессы

На предыдущем уроке мы настроили графики Отчетов по данным. Сегодня мы разберем, что такое модуль BPM-процессов.

### Обдумываем

Спросите у коллег какая работа занимает много времени. Эти процессы можно перенести и автоматизировать в Бипиуме. Например, можно автоматизировать постановку и планирование задач сотрудников. Бывает, сотрудник фиксирует информацию по новому клиенту на первом попавшемся листке. Затем этот листок попадает в кипу других бумаг и теряется. Когда о нём вспоминают, вопрос уже не актуален. Чтобы не терять информацию, нужно сразу планировать задачи по новому клиенту.

Сегодня мы создадим BPM-сценарий, который будет ставить сотруднику задачу «Позвонить» новому клиенту.

***

### Воплощаем

Настроим BPM-сценарий «Позвонить». Сценарий будет запускаться при добавлении нового клиента. Для этого создадим Событие, по которому будет запускаться процесс. Процесс создаст новую «Задачу», подтянет в неё данные клиента, задаст сроки и свяжет с карточкой клиента.

1. **Создадим каталог ЗАДАЧИ** и настроим карточку из полей: поле Текст назовем «Задание», поле Связанный объект — «Компания», поле Дата (с отображением времени) — «Когда», поле Сотрудник — «Ответственный».Другие поля добавьте по вкусу.
2. **Создадим Событие**, которое будет следить за созданием записей в каталоге Компании. Для этого заходим в отдел Управление и в каталоге «События» нажимаем «Добавить». Вводим название события, у нас это будет «Задание менеджеру» при создании клиента. Выбираем каталог, в котором будем отслеживать изменения — «Компании». Выбираем тип событие — «Уведомление о создании записи». В поле «Действия» указываем сценарий, который необходимо выполнить. В выпадающем списке поля «Выбрать» выберем — «добавить в Сценарии». Присваиваем название сценарию, например: «Выявить потребности» и сохраним. Настроим сценарий чуть позже.

{% embed url="https://vimeo.com/1024668845" %}

_Подробнее о Событиях описано здесь:_ [_docs.bpium.ru/processes/events_](https://docs.bpium.ru/processes/events)

3. **Определим API полей.** Заходим в настройки каталога КЛИЕНТЫ. Под каждым полем указан номер API поля. У меня это, API: 8 — «Дата следующего контакта» и API: 7 — «Сотрудник». Записываем номера полей — данные из которых хотим использовать в будущей Задаче. Заходим в каталог ЗАДАЧИ и так же смотрим номера полей — куда мы хотим записывать данные. У меня это API: 2, 3, 4 и 5.

{% embed url="https://vimeo.com/1024668863" %}

4. **Настроим сценарий.** Переходим в каталог СЦЕНАРИИ и выбираем только что созданный сценарий — «Выявить потребности». В поле «Сценарий» выберем кнопку — «Создать», откроется редактор сценариев. Сценарий запускается с входными параметрами:\
   \
   catalogId — каталог, в котором создана запись (у нас ID каталога Клиенты)\
   recordId — ID новой созданной записи (ID записи Клиента)\
   values — массив значений полей карточки созданного клиента\
   \
   Эти переменные будем используются в сценарии для создания карточки Задачи.\
   \
   Как работать со сценариями описано здесь: [docs.bpium.ru/processes/scripts](http://docs.bpium.ru/processes/scripts)
5. **Создадим Задачу в сценарии.** Из левой части экрана перетащим в основное окно компонент «Создать запись» и установим его между началом и концом процесса. Нажимаем на компонент «Создать запись», в правой части экрана откроется карточка компонента, заполним её. В поле «Каталог» выбираем каталог, в котором будет создаваться новые записи — ЗАДАЧИ. Ниже заполняем поле «Значения полей». Каждая строка этого списка состоит из 2 частей: первая часть — номер API поля в карточке Задачи, второе — значение, которое должно быть туда записано. Это может быть текст, дата или значение из переменной. Например, из переменной VALUES, в которой содержаться значения полей карточки созданного Клиента. У нас получилось так:\
   \
   2 = "Позвонить"\
   3 = \[{catalogId, recordId}]\
   4 = values\[8]\
   5 = values\[7]\
   \
   Почему именно такие значения, читайте в описании компонента «Создать запись»: [docs.bpium.ru/processes/scripts/components/createrecord](http://docs.bpium.ru/processes/scripts/components/createrecord)
6. _**Сохраняем сценарий и запись.**_ После сохранения сценария, не забудьте сохранить саму запись. Для запуска сценария добавьте запись в каталог КЛИЕНТЫ. При сохранении, система запустит сценарий «Выявить потребности», который создаст задачу.

{% embed url="https://vimeo.com/1024668885" %}

Сегодня мы настроили BPM-сценарий, который создает задачу сотруднику при сохранении нового клиента. На следующем уроке я расскажу как настраивается интеграция с любой сторонней системой.

_Более подробная информация о BPM-сценариях здесь_ [_docs.bpium.ru/processes_](http://docs.bpium.ru/processes)
