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

# Урок 7. Права доступа к данным

На предыдущем уроке мы пригласили сотрудников в компанию. Сегодня мы настроим права доступа к информации.

### Обдумываем

Каждый сотрудник в компании имеет свою роль, то есть свою зону ответ-ственности и свой уровень доступа к информации. Например: сотрудников отдела продаж можно объединить в группу с одинаковой должностью — Специалистов по продажам.

В Бипиуме, мы объединим одинаковых специалистов в группы и присвоим им должность = роль. Права мы будем назначать на роли, это позволит автоматически распространять права сразу на группу сотрудников.

***

### Воплощаем

1. **Создадим каталог РОЛИ** в отделе Управление. Создайте карточку Роли из необходимых полей, но важно, чтобы первое было — Текстовое поле «Должность». Создайте несколько ролей.
2. **Создадим новое поле «Должность»** в карточке СОТРУДНИКА. Оно будет отображать роль сотрудника в компании. Для этого используем элемент «Связанный объект», присваиваем полю название — «Должность» и связываем его с каталогом РОЛИ.
3. **Назначим роли между сотрудниками.** Например: у Иванова Ивана должность — «Специалист продаж».

{% embed url="https://vimeo.com/1024668939" %}

4. **Настроим права доступа на каталог КОМПАНИИ.** Перейдите в каталог КОМПАНИИ и откройте «Доступ к каталогу» — это иконка шестеренки около названия каталога. Перед вами откроется окно настройки доступа. Выберите сотрудника или должность и назначьте привилегию. Например, специалистам по продажам мы разрешим «Создавать записи», но не экспортировать. Каждая из разрешающих привилегий включает возможности предыдущих.
5. **Настроим права на Вид.** Если вам нужно разграничить доступ к информации в пределах одного каталога, настроим фильтр по полю — «Ответственный сотрудник», выберем значение \[Сотрудник.Я]. Сохраним новый вид, как Правовой. При сохранении, система откроет нам окно настройки доступа. Выберите необходимую роль или сотрудника и присвойте им привилегии из выпадающего списка.

Сегодня мы изучили небольшую часть Правовой политики Бипиума, о том, как раздавать права доступа на сотрудников и группы сотрудников. На следующем уроке я расскажу, как настраивать графические отчеты и воронку продаж.

_Подробнее о Правовой политике найдете здесь:_ [_docs.bpium.ru/rights_](http://docs.bpium.ru/rights)
