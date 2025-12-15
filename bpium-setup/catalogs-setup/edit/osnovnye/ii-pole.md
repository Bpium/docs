---
description: >-
  Текстовое поле с расширенными настройками. Поле позволяет обращаться к большим
  языковым моделям(LLM) и записать результат запроса в формате текста.
---

# ИИ-поле

## Параметры

В параметрах отображаются элементы поля, которые необходимо настроить.

<figure><img src="../../../../.gitbook/assets/Parameters.png" alt=""><figcaption></figcaption></figure>

**Способ ввода.** Выбор варианта заполнения текстового поля. В данном случае необходимо выбрать "ИИ-запрос".

<figure><img src="../../../../.gitbook/assets/Input_variants.png" alt=""><figcaption></figcaption></figure>

**Сервис.** Выбор  языковой модели для обращения. Подключиться можно к одной из следующих моделей: `YandexGpt(Бесплатно от Бипиум), YandexGpt, GigaChat, Deepseek, Grok, Sonar(Perplexity)`.

<figure><img src="../../../../.gitbook/assets/Services.png" alt=""><figcaption></figcaption></figure>

**Модель.** Позволяет указать модель для подключения, выбрав её из списка или через переменную.

<figure><img src="../../../../.gitbook/assets/Models.png" alt=""><figcaption></figcaption></figure>

**Авторизационный токен.** Позволяет указать авторизационный токен для подключения через переменную или выбрать его из списка уже добавленных в каталог Доступы к сервисам.

<figure><img src="../../../../.gitbook/assets/Auth_token.png" alt=""><figcaption></figcaption></figure>

**Идентификатор каталога Yandex Cloud (для Сервиса YandexGPT).** Позволяет указать идентификатор каталога Yandex Cloud через переменную или выбрать из списка уже добавленных в каталог Доступы к сервисам.

<figure><img src="../../../../.gitbook/assets/yandex_dir_id.png" alt=""><figcaption></figcaption></figure>

**Промпт.** Основной текстовый запрос(инструкция) для языковой модели. Подробно опишите задачу, которую должен выполнить ИИ.

<figure><img src="../../../../.gitbook/assets/Prompt.png" alt=""><figcaption></figcaption></figure>

## Пример работы

После настройки в карточке в нужном поле появится кнопка "Сгенерировать"

<figure><img src="../../../../.gitbook/assets/work_button.png" alt=""><figcaption></figcaption></figure>

При нажатии произойдёт генерация ФИО в дательном падеже.

<figure><img src="../../../../.gitbook/assets/work_result.png" alt=""><figcaption></figcaption></figure>

**Обратите внимание** ИИ-поле передаёт весь контекст карточки в запрос. В примере выше сервис YandexGPT получит весь набор полей с их названиями и сам поймёт, что из переданного является ФИО.
