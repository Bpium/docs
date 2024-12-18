# Назначение переменных

Используется для сохранения значений в переменные на определенном этапе выполнения сценария.

## Свойства

### Секция «Назначение переменных»

**Переменные**  \
Выходной параметр. Набор переменных и их значений. Слева указывается название [переменной](https://docs.bpium.ru/processes/scripts/variables), справа — её значение. Значение может быть указано в виде текста (для этого его нужно обернуть в "кавычки") или в формате [выражения](https://docs.bpium.ru/processes/scripts/expression).

{% hint style="info" %}
В качестве переменной  можно указать ключ объекта и данные сохранятся как значения этого ключа.
{% endhint %}

## Пограничные события

![](../../../../.gitbook/assets/boundary_any.png)

Компонент поддерживает 2 типа пограничных событий:

* Ошибка — выход из компонента, если произошла какая-либо ошибка
* Таймаут — выход из компонента, спустя заданное ограничение по времени

Если компонент завершился с ошибкой, но на нем не было пограничного события, то процесс завершается. Сообщение ошибки возвращается в результатах процесса.