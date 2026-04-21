# Поля

Каждое поле имеет тип — он определяет формат хранимых данных и то, как поле выглядит в анкете. Все типы полей разделены на пять групп по назначению.

## Группы полей

<details>

<summary><a href="osnovnye/">Основные:</a></summary>

Универсальные поля для хранения базовых данных: чисел, текста, дат, файлов. Сюда же входят структурные элементы анкеты — секции и вкладки.

* [Секция и вкладки ](osnovnye/sekciya-i-vkladki.md)
* [Число](osnovnye/chislo.md)
* [Текст](osnovnye/tekst.md)
* [Текстовый редактор](osnovnye/tekstovyi-redaktor.md)
* [ИИ-поле](osnovnye/ii-pole.md)
* [Дата](osnovnye/data.md)
* [Файл](osnovnye/fail.md)

</details>

<details>

<summary><a href="kontaktnye-dannye/">Контактные данные:</a></summary>

Поля для хранения контактной информации.

* [Телефон](kontaktnye-dannye/telefon.md)
* [Электронная почта](kontaktnye-dannye/elektronnaya-pochta.md)
* [Сайт / Ссылка](kontaktnye-dannye/sait-ssylka.md)
* [Адрес](kontaktnye-dannye/adres.md)

</details>

<details>

<summary><a href="ocenochnye/">Оценочные:</a></summary>

Поля для оценки состояния или степени чего-либо. Удобны для отслеживания этапов, прогресса и субъективных оценок.

* [Статус](ocenochnye/status.md)
* [Набор галочек](ocenochnye/nabor-galochek.md)
* [Переключатель](ocenochnye/pereklyuchatel.md)
* [Прогресс](ocenochnye/progress.md)
* [Оценка звёздами](ocenochnye/ocenka-zvyozdami.md)

</details>

<details>

<summary><a href="variantivnye/">Вариативные</a><a href="ocenochnye/">:</a></summary>

Поля для выбора значений из предопределённого набора или из данных другого каталога. Используются для фильтрации, связей между каталогами и сегментации данных.

* [Выбор значения](variantivnye/vybor-znacheniya.md)
* [Выпадающий список](variantivnye/vypadayushii-spisok.md)
* [Связанный каталог](variantivnye/svyazannyi-katalog.md)
* [Сотрудник](variantivnye/sotrudnik.md)

</details>

<details>

<summary><a href="deistviya/">Действия:</a></summary>

Интерактивные элементы анкеты. Кнопка позволяет запускать сценарии автоматизации прямо из карточки записи.

* [Кнопка](deistviya/knopka.md)

</details>

## Действия с полем в конструкторе

Для каждого поля в конструкторе доступен набор базовых действий. Чтобы открыть настройки поля, нажмите на него — справа откроется панель свойств.

**Именование поля.**

Введите название прямо в заголовок поля в анкете. Название отображается сотрудникам при заполнении записи.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Заголовок. Наименование поля.</p></figcaption></figure>

**Выбор обязательного поля**. Нажмите на иконку звездочки рядом с полем, чтобы сделать его обязательным. Система не позволит создать запись, пока это поле не заполнено.

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Обязательное для заполнение поля.</p></figcaption></figure>

**Удаление поля.** Кнопка с иконкой корзины предназначена для удаления поля из структуры каталога.

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Удаление поля из структуры каталога.</p></figcaption></figure>

**Перетаскивание поля.** При необходимости поменять порядок полей в записи нажмите на иконку поля и потяните поле вверх или вниз.

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Иконка поля. Изменение порядка полей.</p></figcaption></figure>

**Настройка свойств.** Панель с настройками позволяет настроить поле под свои нужды. Свойства бывают общие для всех полей (подробнее в [Свойства](../svoistva.md)) и индивидуальные для каждого поля (подробнее в статье каждого поля).&#x20;

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Общие свойства полей.</p></figcaption></figure>

