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

# Урок 9. Отправка данных на сторонние сервисы

Bpium может не только получать данные, но и отправлять их в другие системы. Разберем на примере как это можно реализовать.

{% embed url="https://www.youtube.com/watch?ab_channel=Bpium&v=eMqb6MYHx0w" %}

## Кейс

Bpium с помощью компонента веб-запрос шлет необходимые данные на сторонние сервисы.

## Навыки

В этом уроке будут рассмотрены следующие **компетенции** и их навыки:

* **Принципы Бипиума:** понимание принципа веб-технологий
* **Сценарий:** выход по таймауту
* **Компоненты:** веб-запрос
* **Веб-технологии:** внешние запросы, http запросы

***

## В этом уроке

#### **Демонстрация: Описание кейса (0:15)**

Bpium с помощью компонента веб-запрос шлет необходимые данные на сторонние сервисы.

#### **Демонстрация: Создание события (0:38)**

1. В отделе «Управление» в каталоге «События» добавьте новую запись (событие, по которому будет запускаться процесс).
2. Укажите название. Например, ту задачу, которую выполняет процесс.
3. Выберите каталог, в котором хотите отслеживать изменение записей.
4. Выберите тип события для запуска сценария.&#x20;
5. В поле «Выполнить» выберите или создайте новый сценарий.

#### **Демонстрация: Создание сценария (1:10)**

1. В отделе «Управление» в каталоге «Сценарии» добавьте новую запись.
2. Укажите название сценария и нажмите «создать» в поле «Сценарий».
3. Нарисуйте сценарий: из панели компонентов выберите требуемые и расположите их в необходимой последовательности, задайте их свойства.

Компоненты:

* **Веб-запрос** — используется для выполнения http-запросов к произвольной внешней системе.

***

## Домашнее задание

Для расширения знаний и подготовки к следующим урока изучите материалы:

* Описание компонента "Веб-запрос"\
  [https://docs.bpium.ru/processes/scripts/components/webrequest](https://bpium.ru/learn-scripts/lesson-9)