---
description: >-
  Бипиум автоматически рассчитает по вашим правилам индивидуальные скидки для
  клиентов.
---

# Расчет скидок для клиентов

## 1. **Введение**

Вы рассчитываете индивидуальные скидки для своих клиентов. Когда клиентов немного и возможно держать в голове каждого из них — процесс идет налажено. Но, когда клиентов становится больше, становится невозможно запомнить размер скидки каждого из них. Функция Бипиума исключает влияние человеческого фактора и автоматизирует процесс расчета и начисления индивидуальных скидок.

## **2. Принцип работы**

<figure><img src="../../.gitbook/assets/1 (10).png" alt=""><figcaption></figcaption></figure>

Принцип работы основан на каталоге с правилами скидок для каждого клиента. При сохранении записи в каталоге «Покупки» («Заказы») сценарий рассчитывает итоговую сумму заказа с учетом и без учета персональной скидки. Если в каталоге с правилами скидок есть персональная скидка для выбранного клиента, то итоговая сумма снижается на величину скидки.

## **3. Реализация**

### **3.1. Создание каталогов**

Дальнейшее описание подразумевает то, что у вас уже есть каталог с вашими клиентами. Если его еще нет – создайте каталог «Клиенты» и заполните его произвольным образом.

#### **3.1.1. Каталог с правилами скидок**

Создайте каталог «Правила скидок». В нём будут храниться проценты скидок по каждому клиенту. \
****\
****Структура каталога «Правила скидок»:

<figure><img src="../../.gitbook/assets/2 (9).png" alt=""><figcaption></figcaption></figure>

* **Клиент** (связанный каталог)\
  Описание: Клиент, для которого будет указана скидка.\
  Настройки: связанный каталог «Клиенты», обязательное поле.
* **Скидка** (число)\
  Описание: Величина скидки в процентах.\
  Настройки: Обязательное поле.

#### 3.1.2. **Каталог с товарами**

Создайте каталог «Товары». В этом каталоге фиксируются наименование товара и его стоимость. При необходимости можете добавить в него другие произвольные поля.

<figure><img src="../../.gitbook/assets/3 (9).png" alt=""><figcaption></figcaption></figure>

* **Наименование** (текст)\
  Описание: Наименование товара.\
  Настройки: текст, обязательное поле.
* **Стоимость** (число)\
  Описание: Стоимость за 1 единицу товара.\
  Настройки: обязательное поле.

#### 3.1.3. **Каталог с количеством товаров**

Создайте каталог «Товары и количество». В этом каталоге указывается товар и его количество. Каталог является связующим между каталогами «Товары» и «Заказы» (описан ниже). Заполните каталог следующими полями:

<figure><img src="../../.gitbook/assets/8 (7).png" alt=""><figcaption></figcaption></figure>

* **Товар** (связанный каталог)\
  Описание: Поле для выбора товара.\
  Настройки: Связь с каталогом «Товары», обязательное поле.
* **Количество** (число)\
  Описание: Количество заказываемого товара.\
  Настройки: Обязательное поле.

#### **3.1.4. Каталог с заказами**

Создайте каталог «Заказы». В этом каталоге будет собрана информация о всех заказах клиентов. Заполните каталог следующими полями:

<figure><img src="../../.gitbook/assets/4 (7).png" alt=""><figcaption></figcaption></figure>

* **Клиент** (связанный каталог)\
  Описание: Клиент, оформляющий заказ.\
  Настройки: Связанный каталог «Клиенты», обязательное поле.
* **Товары** (связанный каталог)\
  Описание: Приобретаемый товар.\
  ****Настройки. **** Выберите связанный каталог «Товары и количество». Поставьте галочку на «Можно связывать несколько записей». Снимите «галочку» с «Можно выбирать из существующих». Поставьте «галочку» на «Создание без всплывающего окна». В «Поля» выберите «Товар» и «Количество», доступ – «изменять». Обязательное поле.
* **Количество** (число)\
  Описание: Количество приобретаемого товара.\
  Настройки: Обязательное поле.
* **Сумма** (число)\
  Описание: Сумма без учета скидки.\
  Настройки: Редактируемое только через API.

### **3.2. Событие для запуска расчета**

Для запуска сценария расчета суммы заказа используются событие с типами «Запрос на создание записи» и «Запрос на изменение записи». Это событие отслеживает сохранение записи при изменении значений полей, которые участвуют в расчете.&#x20;

Пример события:

<figure><img src="../../.gitbook/assets/5 (9).png" alt=""><figcaption></figcaption></figure>

При изменении полей «Клиент» или «Товары» в записи «Заказа» событие запускает сценарий расчета сумм для клиента. Прикрепите [файл сценария](https://drive.google.com/file/d/1MB4JkNxEhDpafK\_umFXUtE5Jf6Or-HDA/view?usp=sharing) в поле «Выполнить» созданного события.

### **3.3. Сценарий расчета сумм для клиента**

Сценарий расчета сумм для клиента выглядит следующим образом:

<figure><img src="../../.gitbook/assets/6 (8).png" alt=""><figcaption></figcaption></figure>

Сценарий выполняет:

* Проверку на заполненность полей «Клиент» и «Товар».
* Поиск индивидуальной скидки для клиента в каталоге «Правила скидок».
* Подсчет полной суммы заказа [в цикле](https://docs.bpium.ru/manual/processes/scripts/cases/for).
* Подсчет суммы заказа с учетом скидки.
* Запись в карточку заказа полной суммы и суммы с учетом скидки.

В сценарии необходимо изменить выделенные компоненты:

* params. Замените id полей в объекте «fields» согласно описанию компонента.

## **4. Тестирование**

Создайте новые записи в каталогах «Клиенты», «Правила скидок», «Товары».&#x20;

В каталоге «Заказы» выберите клиента (с правилом скидки), добавьте товары и укажите их количество. Если все сделано верно, то при сохранении записи поля «Сумма» и «Сумма (со скидкой)» автоматически заполнятся.

<figure><img src="../../.gitbook/assets/7 (8).png" alt=""><figcaption></figcaption></figure>