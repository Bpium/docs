# Варианты разворачивания

## **Общая структура** <a href="#id-19ev1v8ycb0r" id="id-19ev1v8ycb0r"></a>

![](<../../../.gitbook/assets/0 (2).jpeg>)

Bpium Enterprise состоит из 3 независимых приложений:

* **Bpium** — сервер приложения и логики
* **Bpium S3** — сервер файлового хранилища
* **Bpium BPM** — сервер исполнения процессов

Также для работы используются другие приложения:

* **Postgre SQL** — сервер баз данных
* **Redis**— сервер хранилища состояния процессов и очереди заданий

Клиенты для работы сотрудников:

* **Веб-приложение (браузер)** — приложение для настройки системы и работы сотрудников
* **Мобильное приложение** — приложение для работы сотрудников

Серверные приложения, база данных и файловое хранилище могут быть установлены на одном или разных серверах, работать на защищенном канале связи HTTPS или незащищенном HTTP.

## Варианты запуска и требования

<details>

<summary><a href="https://docs.bpium.ru/~/revisions/PCOuurHnldwBOuxvsJ9t/deployment/skhemy-razvorachivaniya/varianty-razvorachivaniya/minimalnyi-nabor-dlya-testovogo-zapuska">Минимальный набор для тестового запуска</a></summary>



</details>

<details>

<summary><a href="https://docs.bpium.ru/~/revisions/7xZbuF5sSwWydmpdgEBU/deployment/skhemy-razvorachivaniya/varianty-razvorachivaniya/rekomenduemyi-nabor-dlya-rabochego-production-zapuska-bipium">Рекомендуемый набор для рабочего (production) запуска Бипиум</a></summary>



</details>
