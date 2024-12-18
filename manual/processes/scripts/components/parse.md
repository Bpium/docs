# Парсер

Используется для поиска и получения значения из текстового массива данных.

## Свойства

### Секция «Найти»

**Данные**  \
Строка с текстовыми данными, в которой требуется найти нужный фрагмент. Данными могут быть любого формата. Формат: «значение в кавычках» или выражение.

**Метод поиска**  \
Способ поиска вхождений в исходной строке. Доступны следующие: `JSONPath, XPath, jQuery-селектор, Regex` .

* JSONPath используется для поиска элементов в JSON-объекте и JS-объекте.
* XPath используется для поиска элемента в XML или HTML-документе.
* jQuery-селектор используется для поиска элемента или данных в HTML-документе.
* Regex используется для поиска с помощью регулярных выражений.

**Поисковая строка**  \
Выражение, описывающее поисковую строку. Используется различный формат для разных методов поиска:

* JSONPath, [примеры использования](https://github.com/s3u/JSONPath), пример ввода: "$.store.book\[\*].author"
* XPath, пример ввода: "/element/" .
* jQuery-селектор, поддерживает все [jquery-селекторы](https://www.w3schools.com/jquery/jquery_ref_selectors.asp), пример ввода: "#elementid", ".classname", "p:first"
* Regex, пример ввода: "/\[0-9]+/".

Формат: «значение в кавычках» или выражение.

**Возвращаемое значение** (для jQuery-селектор)  \
Свойство доступно при выборе метода поиска «jQuery-селектор». Указывает, что именно вернуть у найденного элемента:

* HTML данные внутри элемента (html)
* Текст внутри элемента (text)
* Значение элемента (val), используется для input-полей
* Элемент
* Атрибут (attr)

**Возвращаемое значение** (для Regex)\
Свойство доступно при выборе метода поиска «Regex» Регулярное выражение может содержать несколько поисковых подстрок (заключенных в круглые скобки). Свойство указывает индекс поисковой подстроки (по умолчанию 0). Формат: значение/выражение.

**Вернуть**  \
Указывает какое из найденных значение вернуть. Можно вернуть `первый элемент`, `последний элемент` или `все`.

### Секция «Результат»

**Сохранить в**  \
Выходной параметр. Сохранит найденные данные в переменную. Формат: имя переменной.

{% hint style="info" %}
В «**Сохранить в**» можно указать ключ объекта и найденные данные сохранятся как значения этого ключа.
{% endhint %}

## Пограничные события

![](../../../../.gitbook/assets/boundary_any.png)

Компонент поддерживает 2 типа пограничных событий:

* Ошибка — выход из компонента, если произошла какая-либо ошибка
* Таймаут — выход из компонента, спустя заданное ограничение по времени

Если компонент завершился с ошибкой, но на нем не было пограничного события, то процесс завершается. Сообщение ошибки возвращается в результатах процесса.