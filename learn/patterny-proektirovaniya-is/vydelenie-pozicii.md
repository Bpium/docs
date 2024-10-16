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

# Выделение позиций

## Описание

{% hint style="success" %}
Выносите позиции бизнес-объектов в отдельный каталог
{% endhint %}

## Проблематика

Рассмотрим проблемы несоблюдения _паттерна выделения позиций_ на примере системы учета заказов, спроектированной на Бипиуме.

Заказ состоит из номенклатуры с указанием товаров и их количества. В одном заказе может быть номенклатура из нескольких различных товаров.

Если при проектировании не использовался [_паттерн разделения_](razdelenie.md) _и выделение позиций_, то в системе будет настроен единственный каталог - "Заказы":

<figure><img src="../../.gitbook/assets/Untitled (1).png" alt=""><figcaption></figcaption></figure>

На примере настроенного каталога можем выделить следующие проблемы:

* Ручное заполнение полей идентичными значениями
* Усложнение фильтрации и ведения отчетности
* Невозможность формирования динамического количества позиций

### Ручное заполнение полей идентичными значениями

В настроенном каталоге есть поля, которые заполняются текстовым значением вместо использования связей с дополнительными каталогами:

* Клиент
* Товар 1
* Товар 2

Например: по одному клиенту в системе может быть несколько заказов. Во втором заказе по клиенту нужно вновь указывать его наименование вручную.

Такой подход увеличивает время заполнения полей в записи: каждое поле заполняется вручную вместо выбора уже заполненного ранее значения.



Кроме этого при добавлении дополнительных полей связанных с клиентом или товарами карточка заказа будет увеличиваться, затрудняя навигацию.

### Усложнение фильтрации и ведения отчетности

Из-за ручного ввода значений вероятность орфографических ошибок многократно увеличивается. В связи с этим возможна некорректная фильтрация записей в каталоге.

Например, по одному и тому же клиенту заведено 2 заказа. В одном из них допущена орфографическая ошибка в наименовании клиента:

<div>

<figure><img src="../../.gitbook/assets/correct.jpg" alt=""><figcaption><p><mark style="color:green;">Здесь наименование заполнено корректно</mark></p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/incorrect (1).jpg" alt=""><figcaption><p><mark style="color:red;">А здесь в наименовании допущена ошибка:</mark></p></figcaption></figure>

</div>

При попытке отфильтровать все заказы по клиенту система выдаст только один заказ, хотя фактически по клиенту сформировано два заказа:

<figure><img src="../../.gitbook/assets/filter1.png" alt=""><figcaption></figcaption></figure>

Также будет затруднена фильтрация всех заказов по определенному товару. Предположим, что во втором заказе клиент хочет заказать те же товары, но названия товаров теперь расположены в других полях:

<div>

<figure><img src="../../.gitbook/assets/filterExmp1.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/filterExmp2.png" alt=""><figcaption></figcaption></figure>

</div>

При попытке получить все заказы с монитором получаем только один заказ вместо двух:

<figure><img src="../../.gitbook/assets/filter2.png" alt=""><figcaption></figcaption></figure>

Такое происходит из-за того, что во втором заказе монитор указан в поле “Товар 2”, а не “Товар 1”.

То есть для того, чтобы получить общее количество заказов с определенным товаром нужно последовательно вставить наименование товара в каждое поле и сложить общее количество записей во всех выдачах. Такой подход многократно увеличивает время на анализ данных и делает невозможным ведение отчетности в виде графиков в системе.

### Невозможность формирования динамического количества позиций

В примере рассмотренном выше есть возможность заполнения номенклатуры, состоящей из двух товаров. При этом, если клиент закажет три разных товара, то в системе не будет возможности указать все три в рамках одной записи.

Для того, чтобы сформировать такой заказ:

* Сотрудникам необходимо создать дополнительную запись и дозаполнить в ней “лишний” товар.
* Разработчику системы нужно добавлять новые поля для каждого нового случая увеличения товаров в номенклатуре.

### Невозможность добавления одного товара в заказы в разных количествах

Мы часто сталкиваемся с тем, что пользователи применяют [паттерн разделения](razdelenie.md) и "Товар" выводится в отдельный бизнес-объект, но при этом "Количество" определяют как атрибут товара.

Рассмотрим реализацию того же функционала формирования заказов в примере, когда "Количество" является атрибутом "Товара":

<figure><img src="../../.gitbook/assets/goods.png" alt="" width="470"><figcaption></figcaption></figure>

Предположим, что другой клиент также заказывает кресло, но в ином количестве.

<figure><img src="../../.gitbook/assets/goodsCount2.png" alt="" width="470"><figcaption></figcaption></figure>

По причине того, что мы берем тот же товар из того же каталога и укажем другое количество, у нас перезапишется количество товара и в первом заказе.

<figure><img src="../../.gitbook/assets/goodsCount1.png" alt="" width="469"><figcaption></figcaption></figure>

Действие вполне логичное, но в нашем контексте абсолютно некорректное.

В общем случае подобная ситуация может привести ко многим проблемам, связанным с несоответствием текущего состояния записей их фактическому состоянию, а именно к убыткам организации или снижения доверия к ней со стороны подрядчиков и клиентов.

## Применение паттерна

Теперь рассмотрим реализацию системы учета заказов с использованием _паттерна разделения и выделения позиций_:

<figure><img src="../../.gitbook/assets/addLinkedCat.png" alt=""><figcaption></figcaption></figure>

Были добавлены каталоги:

* Клиенты: каталог с базой всех клиентов в системе

<figure><img src="../../.gitbook/assets/client.png" alt=""><figcaption></figcaption></figure>

* Товары: каталог с базой товаров, доступных к заказу

<div>

<figure><img src="../../.gitbook/assets/chair.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/monitor.png" alt=""><figcaption></figcaption></figure>

</div>

* Позиции заказа: каталог-прослойка с указанием товара и его количества для заказа

<div>

<figure><img src="../../.gitbook/assets/position1.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/position2.png" alt=""><figcaption></figcaption></figure>

</div>



{% hint style="info" %}
В поле "Позиции" каталога "Заказы" выведены расширенные поля с ценой, количеством и стоимостью для большей наглядности.
{% endhint %}

В данной реализации можно добавлять одни и те же товары в различных количествах к разным заказам. При этом мы избегаем проблемы перезаписи количества товара в одном заказе, когда добавляется товар в другой заказ.

Добавлять новые позиции товаров можно прямо внутри записи заказа

<figure><img src="../../.gitbook/assets/addPosistions.png" alt="" width="470"><figcaption></figcaption></figure>

Также появляется возможность добавлять новые товары непосредственно в записе заказа, если они по какой-то причине не были добавлены ранее. Это экономит наше время, избавляя от необходимости переходить в другой каталог для создания нового товара.