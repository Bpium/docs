# Компоненты

Bpium для описания сценариев использует нотацию BPMN 2.0. Сценарии состоят из связанных между собой компонентов: событий, шлюзов и действий. Компоненты обмениваются данными между собой через переменные.

<figure><img src="../../../../.gitbook/assets/firstScript.png" alt=""><figcaption></figcaption></figure>

## Компоненты

### События (Events)

События – это то, что происходит в течение процесса. События оказывают влияние на ход процесса. Изображаются в виде круга с изображением типа события. Согласно влиянию событий на ход процесса, выделяют три типа: стартовое событие (Start), промежуточное событие (Intermediate) и конечное событие (End). Промежуточные события могут быть прикреплены к другим компонентам. Например, к действию может быть прикреплено событие ошибка.

* [Начало процесса](start.md)
* [Конец процесса](finish.md)
* [Таймер](timer.md)

### Действия (Activities)

Действия – общий термин, обозначающий некую работу, выполнение некой задачи. Изображаются в виде прямоугольников с изображением типа операции. В Бипиуме среди действий отдельно выделены компоненты для работы с данными Bpium.

#### Действия с данными Bpium

* [Получить запись](getrecord.md)
* [Найти записи](findrecords.md)
* [Изменить запись](editrecord.md)
* [Создать запись](createrecord.md)
* [Удалить запись](deleterecord.md)
* [Структура каталога](poluchenie-struktury-kataloga.md)
* [Загрузить файл](zagruzit-fail.md)
* [Сгенерировать документ](generaciya-dokumenta.md)

#### Действия

* [Назначение переменных](setvariables.md)
* [Код (JavaScript)](code.md)
* [Веб-запрос](webrequest.md)
* [SQL-запрос](sql.md)
* [Парсер](parse.md)
* [Запуск процесса](zapusk-processa.md)
* [Отправка почты](email.md)

### Пограничные события (Boundary events)

Пограничные события — альтернативные варианты завершения компонентов-действий. Эти события «вешаются» на сам компонент-действие и активируют альтернативные выходы из компонента при определенных событиях.

![](../../../../.gitbook/assets/boundary.png)

Сценарии Бипиум поддерживают 2 типа пограничных событий:

* [Ошибка](error.md) — выход из компонента, если произошла какая-либо ошибка
* Таймаут — выход из компонента, спустя заданное ограничение по времи

Если компонент завершился с ошибкой, но на нем не было пограничного события, то процесс завершается. Сообщение ошибки возвращается в результатах процесса.

### Шлюзы (Gateways)

Шлюзы используются для контроля расхождений и схождений потока исполнения в процессе: ветвление, распараллеливание, слияние и соединение маршрутов. Изображаются в виде ромба с изображением типа развития процесса.

* [Шлюз «ИЛИ» (условное ветвление)](exclusivegateway.md)
* [Шлюз «И» (распараллеливание)](parallelgateway.md)

### **Соединяющие линии**

Компоненты процесса связаны друг с другом соединяющими линиями. Из компонента могут выходить несколько линий — процесс пойдет одновременно по всем из них. На соединяющие линии можно задать условия, в этом случае процесс перейдет к следующему компоненту только при выполнении условия.

* [Соединяющая линия](line.md)

## Свойства компонентов

Компоненты имеют свойства, задающие параметры работы компонента. Все свойства можно разделить на два типа: входные параметры и выходные параметры.

### Входные параметры

Входные параметры поступают на вход компонента. Значения входных параметров можно задавать явными значениями (числа и "строки"), указывать [переменные](../variables.md) или [выражения](../expression.md).

Переменные могут быть числами, текстом, датой, массивами и объектами. Подробнее о переменных в соответствующем разделе.

### Выходные параметры

Компоненты передают результаты работы в сценарий через выходные параметры. В качестве значения свойства выходного параметра нужно указывать имя переменной, в которую должен быть сохранен результат. Например, сохранить количество записей в переменную `count`.

{% hint style="info" %}
Вместо имени переменной можно указать свойство существующей переменной. Например, `data.count`. При этом результат компонента будет записан в переменную `data` в свойство `count`. Если переменная `data` не существует, она будет создана.
{% endhint %}
