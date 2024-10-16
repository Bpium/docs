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

# Урок 4. Связи между данными

На предыдущих уроках мы создали структуру будущего решения из отделов и каталогов с карточками внутри. Сегодня мы логически свяжем каталоги между собой, чтобы оперативно находить всю связанную информацию, например с КОМПАНИЕЙ.

### Обдумываем

Когда появился интернет были придуманы гиперссылки. В тексте с помощью них ссылались на компанию или статью. Тем самым информация в интернете, как огромная паутина связана между собой.

В Бипиуме информация тоже связана между собой, это упрощает её поиск и избавляет от проблемы дублирования. Можно не заполнять данные клиента в обращении, а связать его с клиентом из базы. Задачу можно связать с проектом, а проект с клиентом и партнером.

Выясните у коллег, какие данные им часто приходится дублировать или быстро искать и учтите это при настройке Бипиума. Сегодня я покажу вам, как связать карточку Клиента с его контактными лицами.

***

### Воплощаем

1. **Изучим поле «Связанный объект»** в настройках каталога. В отделе ПРОДАЖИ зайдите в настройку заранее созданного каталога КОНТАКТЫ. Элемент «Связанный объект» — связывает записи разных каталогов между собой. Перетащите его в рабочее окно и присвойте ему название — КОМПАНИЯ.
2. **Свяжем поле с другим каталогом.** В выпадающем списке этого элемента выберите заранее созданный каталог — КОМПАНИИ. Можно выбрать и несколько каталогов, чтобы контакт связывать с клиентом или с поставщиком.
3. **Свяжем записи между собой.** Теперь, создавая КОНТАКТ, его можно закрепить его за Компанией. При создании карточки контакта, выберите в поле КОМПАНИЯ, клиента из выпадающего списка. Здесь же, при необходимости можно создать и новую КОМПАНИЮ.

{% embed url="https://www.youtube.com/watch?ab_channel=Bpium&v=8umW3TjXOGY" %}

4. **Отображение связанных объектов.** Заходя в карточку Компании, вы увидите все связанные с ней объекты в конце карточки. Дополнительных связей делать не нужно. Там же можно создать новый Контакт.

Сегодня мы логически связали каталоги между собой. Попробуйте по аналогии связать остальные каталоги в вашем Бипиуме, где это нужно. Если сейчас не знаете где потребуется такая функция, то вы сможете отредактировать карточку в любой момент времени.

_Более подробная информация о Связанном объекте найдете здесь:_\
_Связанный объект —_ [_docs.bpium.ru/structure/catalogs/edit#svyazannyi-obekt_](http://docs.bpium.ru/structure/catalogs/edit#svyazannyi-obekt)\
_Связанные данные —_ [_docs.bpium.ru/structure/records#svyazannye-dannye_](http://docs.bpium.ru/structure/records#svyazannye-dannye)