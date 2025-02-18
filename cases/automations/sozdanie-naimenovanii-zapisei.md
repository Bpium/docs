---
description: Автоматическая генерация наименований записей в Бипиуме.
---

# Создание наименований записей

## **1. Введение**

Вы связываете запись каталога с другой записью и наблюдаете следующее:

<figure><img src="../../.gitbook/assets/1 (4).png" alt=""><figcaption></figcaption></figure>

В списке связанных записей отображаются id записей (#id), а не данные из других полей. Выбирать нужную запись по id – неудобно. Это происходит, потому что в связанном каталоге нет ни одного текстового поля. Бипиум создает наименование записи по первому заполненному текстовому полю, если таких полей нет – наименование создается по id записи.&#x20;

Добавим в каталог текстовое поле, заполним его во всех записях и попробуем выбрать снова:

<figure><img src="../../.gitbook/assets/2 (5).png" alt=""><figcaption></figcaption></figure>

Теперь в списке связанных записей отображаются значения текстового поля.

Если наименования в ваших каталогах должны создаваться по единому шаблону, то можно заполнять их автоматически, используя сценарий. Это освобождает от рутины по заполнению шаблонного поля с наименованием для каждой записи и избавляет от ошибок ввода.

## **2. Принцип работы**

<figure><img src="../../.gitbook/assets/3 (1).jpg" alt=""><figcaption></figcaption></figure>

При сохранении записи в каталоге «Связанный» запускается сценарий формирования наименования. Он автоматически заполняет поле с наименованием из значений других полей по заданному шаблону.

## **3. Реализация**

### **3.1. Настройка структуры каталога**

<figure><img src="../../.gitbook/assets/4 (4).png" alt=""><figcaption></figcaption></figure>

В связанном каталоге, в котором нужно генерировать наименования, создайте текстовое поле и разместите его первым по порядку. Остальные поля каталога могут быть произвольными. Пример структуры каталога:

* **Наименование** (Текст)\
  Описание: Наименование записи. Этот текст будет отображаться в выпадающем списке связанных записей.\
  Настройки: Текст, Редактируемое только через API.
* **Номер** (число)\
  Описание: Произвольное число. Будет автоматически записываться в наименование по шаблону.
* **Статус** (статус)\
  Описание: Произвольный статус записи. Будет автоматически записываться в наименование по шаблону.\
  Настройки: значения – «Активный», «Неактивный».

### **3.2. Настройка автоматизации**

#### **3.2.1. Создание и настройка события**

В системном каталоге «События» создайте новую запись и заполните ее следующим образом:

<figure><img src="../../.gitbook/assets/5 (7).png" alt=""><figcaption></figcaption></figure>

Это событие будет реагировать на изменение полей «Номер» и «Статус» в записи каталога «Связанный» и генерировать наименование записи. В качестве сценария к событию прикрепите [сценарий генерации наименования](https://drive.google.com/file/d/1DnXw_fbHwuHVC8CkTzfHpfsXVi26lTtS/view?usp=sharing).

#### **3.2.2. Сценарий генерации наименования**

Сценарий генерации наименования выглядит следующим образом:

<figure><img src="../../.gitbook/assets/6 (6).png" alt=""><figcaption></figcaption></figure>

**Сценарий выполняет:**

* Объединение измененных и неизмененных значений записи в объект allValues компонентом «\_.assign to allValues». Нужно, чтобы обращаться ко всем значениям полей из одного объекта. [Подробнее](https://docs.bpium.ru/manual/processes/events/datachanged?q=after#uvedomlenie-ob-izmenenii-zapisi-after-update).
* Формирование наименования из значений полей по шаблону в компоненте «Формируем наименование записи».
* Запись наименования в поле «Наименование» текущей записи компонентом «Записываем наименование».

Примечание: в компоненте «Формируем наименование записи» описаны способы формирования наименования по типам полей, не описанным в примере (контакт, связанный каталог и т.д.). Если вы хотите сформировать наименование по другим типам полей, раскомментируйте соответствующие участки с кодом.

В сценарии нужно изменить:

* **Компонент «fields»**\
  Вставьте необходимые id полей и значений в соответствии с описанием внутри компонента.

## **4. Тестирование**

Создайте запись в каталоге «Связанный». Если сценарий и событие настроены верно, то наименование должно автоматически сгенерироваться:

<figure><img src="../../.gitbook/assets/7 (3).png" alt=""><figcaption></figcaption></figure>

Перейдите в запись каталога «Основной» (каталог, в котором вы выбираете связанную запись) и откройте список связанных записей из каталога «Связанный». Вы должны увидеть выборку записей с их наименованиями:

<figure><img src="../../.gitbook/assets/8 (10).png" alt=""><figcaption></figcaption></figure>
