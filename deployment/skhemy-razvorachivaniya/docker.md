---
icon: docker
---

# На одном сервере (Docker)

Bpium Docker — это версия Bpium для установки на собственные сервера или облако. \
Решение включает в себя конфигурационные файлы для создания собственного сервера с технологией контейнеризации.

## Глоссарий

* **Хост** - операционная система, на которой запущены контейнеры **Docker**
* [**Docker**](https://www.docker.com/) - программное обеспечение, реализующее контейнерную изоляцию приложений
* [**Контейнеризация**](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F) -  метод виртуализации, при котором ядро операционной системы поддерживает несколько изолированных экземпляров пространства пользователя вместо одного. Эти экземпляры (обычно называемые контейнерами или зонами) с точки зрения пользователя полностью идентичны отдельному экземпляру операционной системы.

## Архитектура

{% hint style="warning" %}
Внимание: данное руководство содержит инструкцию только по настройке решения на одном сервере.
{% endhint %}

Bpium Docker состоит из независимых контейнеров Docker:

* **Bpium** — сервер приложения и логики
* **Bpium S3** — сервер файлового хранилища
* **Bpium BPM** — сервер исполнения бизнес-процессов
* **PostgreSQL** — сервер баз данных
* **Redis**— сервер хранилища данных для бизнес-процессов

Контейнеры могут быть размещены на одном или разных серверах, включая кластерные решения. Реализация на нескольких серверах аналогична [этой](../architecture.md) и так же потребует индивидуальной настройки.

Если у вас уже есть базы данных и хотите использовать их - это потребует дополнительной настройки файла конфигурации.

Готовая конфигурация включает:

* Операционная система (Alpine)
  * Docker
    * Контейнер Bpium
    * Контейнер BPM
    * Контейнер Bpium-S3
    * Контейнер Postgres
    * Контейнер Redis
  * База данных Postgres:
  * База данных Redis
  * Хранилище Bpium-S3

Сеть устроена следующим образом:

* Контейнеры объединены в одну сеть, имея доступ к друг другу.
* Доступ во внешнюю сеть (интернет) имеют все контейнеры.
* Обслуживать запросы, приходящие на хост могут только те контейнеры, которым это разрешили правилами.

## Подготовка

### Пример файла docker-compose

```
version: "3.8"
volumes: 
  storagebpium:
  redisstorage:
  postgresstorage:
services:
  bpium:
    container_name: bpium
    image: bpiumdocker/bpium
    ports:
      - "80:80"
      - "443:443"
    secrets:
      - cert
      - cert-key
    environment:
      SERIAL_NUMBER: ''
      DB_CONNECTION_STRING: 'postgres://postgres:password@postgres:5432/bpium_database'
      COOKIE_SECRET: ''
      SCRIPTS_TOKEN_SECRET: ''
      S3_HOST: 'внешнедоступный адрес сервера'
      S3_KEY: ''
      S3_SECRET: ''
      BPM_HOST: 'bpm'
      BPM_SECRET: ''
    depends_on:
      - postgres
      - bpm
      - bpium-s3
    logging:
      driver: "json-file"
      options:
        max-size: "200m"
        max-file: "3"
  
  postgres:
    container_name: postgres
    image: "postgres:14-alpine"
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "password"
      POSTGRES_DB: "bpium_database"
      POSTGRES_INITDB_ARGS: "--locale=ru_RU"
      PGDATA: "/postgresdb"
    command: ["postgres", "-c", "log_statement=all", "-c", "log_destination=stderr"]
    volumes:
      - postgresstorage:/postgresdb
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: "200m"
        max-file: "3"

  bpm:
    container_name: bpm
    image: bpiumdocker/bpm
    secrets:
      - cert
      - cert-key
    environment:
      BPM_SECRET: ''
      BPM_QUEUE_HOST: 'redis'
      BPM_QUEUE_LOCAL: 'false'
    depends_on:
      - redis
    logging:
      driver: "json-file"
      options:
        max-size: "200m"
        max-file: "3"

  bpium-s3:
    container_name: bpium-s3
    image: bpiumdocker/s3
    ports:
      - "2020:2020"
    secrets:
      - cert
      - cert-key
    environment:
      S3_HOST: 'внешнедоступный адрес сервера'
      S3_KEY: ''
      S3_SECRET: ''
    volumes:
      - storagebpium:/bpiumpac/storage
    logging:
      driver: "json-file"
      options:
        max-size: "200m"
        max-file: "3"

  redis:
    container_name: redis
    image: "redis:alpine"
    command: > 
      --appendonly yes
      --appendfilename "redisdb.aof"
    volumes:
      - redisstorage:/data
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: "200m"
        max-file: "3"

secrets:
  cert:
    file: ./cert
  cert-key:
    file: ./cert-key
```

### **SSL-сертификаты**

Bpium может работать на защищенном канале связи (HTTPS). \
Для этого сервер приложения должен быть доступен из интернета, иметь доменное имя и SSL-сертификат Class 1 или более высокий.\
Для работы мультидоменной версии Bpium требуется wildcard-сертификат Class 2 или более высокий.

Способы размещения сертификата:

* По-умолчанию:\
  Положите сертификат (.crt) и ключ сертификата в одну директорию с docker-compose.yml и назовите их `cert` и `cert-key` соответственно.
* Свой:\
  Нужно поменять пути для сертификата и ключа в файле docker-compose.yml:\
  `secrets:` \
  &#x20; `cert:` \
  &#x20;   `file: ./cert` \
  &#x20; `cert-key:` \
  &#x20;   `file: ./cert-key`

### Настройка конфигурационного файла (docker-compose.yml)

Настройки Бипиума хранятся отдельно для каждого контейнера и указаны в секции environments, в формате:

`HTTPS: 'true'`\
`SERIAL_NUMBER:'0000-0000-0000-0000'`\
`COOKIE_SECRET:'somestring'`\
`SCRIPTS_TOKEN_SECRET: 'anotherstring'`

{% hint style="warning" %}
Учтите: если ваша конфигурация состоит из нескольких серверов - вам потребуются дополнительные настройки.
{% endhint %}

Список с описанием всех переменных вы можете найти по этим ссылкам:\
[Bpium](https://docs.bpium.ru/server/parametry-config.env/bpium)\
[BPM](https://docs.bpium.ru/server/parametry-config.env/bpium-bpm)\
[S3](https://docs.bpium.ru/server/parametry-config.env/bpium-s3)

### Хранение статичных данных или Persist storage

Одним из минусов контейнеров является то что данные существуют только пока работает контейнер. Это является проблемой, если в контейнере содержится база данных. Для этого случая в Docker есть возможность связывать элементы файловой системы контейнера с файловой системой хоста. Это и позволяет сохранить данные баз данных. Чтобы получить список всех связанных элементов нужно ввести команду: `docker volume ls.`

## Запуск

Процедура запуска контейнеров состоит всего из одного этапа - что является преимуществом контейнерного решения.&#x20;

Прейдите в каталог с файлом `docker-compose.yml` и введите команды, если необходимо - с правами суперпользователя:`docker-compose up -d`\
Это запустит процесс разворачивания необходимых контейнеров согласно настройках в файле docker-compose.yml

Если все сделано верно - то Бипиум готов к работе. Убедимся что контейнеры запущены и работают исправно:`docker ps.`Если все хорошо - вы должны увидеть следующее:

<div align="left"><img src="../../.gitbook/assets/image (55) (1).png" alt="Обратите внимание на статус контейнера. Up - свидетельствует о корректной работе."></div>

Теперь ваш Бипиум должен быть доступен по доменному имени/адресу хоста.\
Сам вход в приложение описан [здесь](https://docs.bpium.ru/server/zapusk#vkhod-v-prilozhenie).

## Обновление

Для обновления приложения Бипиума до последней версии нужно выполнить команду:

```
docker pull bpiumdocker/bpium:latest && docker pull bpiumdocker/bpm:latest && docker pull bpiumdocker/s3:latest 
```

Это запустит процесс синхронизации с хранилищем образов и загрузит самые новые.

После этого нужно перезапустить контейнеры, чтобы Docker использовал новые образы:&#x20;

```
docker-compose up -d
```

## Получение логов

Для диагностики и изучения возможных проблем с Бипиумом вы можете использовать логирование Docker.

Сначала, нужно узнать имя интересующего вас контейнера. \
Для этого введите команду:

```
docker ps
```

Вам будут показаны все контейнеры, включая неработающие.

Для доступа к логам контейнера достаточно ввести следующую команду:

```
docker logs имя_контейнера
```

## Бэкапы, миграции и восстановление

{% hint style="warning" %}
Приложение Bpium самостоятельно не занимается резервированием данных.\
При необходимости, требуется индивидуальная настройка этих процессов.
{% endhint %}

{% tabs %}
{% tab title="PostgreSQL" %}
**Бэкап:**

```
docker exec -i postgres_container_name pg_dump --username pg_username --password pg_password database_name > /path/bpium_dump
```

* `docker exec -i postgres_container_name` - исполняет команду внутри контейнера в интерактивном режиме
* `pg_dump` - утилита для создания бекап-файла данных
* `--username pg_username` - имя пользователя базы данных (по-умолчанию `postgres`)
* `--password pg_password` - пароль пользователя базы данных (см. docker-compose.yml)
* `database_name` - имя базы данных (по-умолчанию `bpium_database`)
* `/path/bpium_dump` - указывает, куда будет сохранен дамп в системе хоста

#### Восстановление:

```
docker exec -i postgres_container_name pg_restore --username pg_username --password pg_password database_name < /path/bpium_dump
```

* `docker exec -i postgres_container_name` - исполняет команду внутри контейнера в интерактивном режиме
* `pg_restore` - утилита для восстановления данных из бекап-файла
* `--username pg_username` - имя пользователя базы данных (по-умолчанию `postgres`)
* `--password pg_password` - пароль пользователя базы данных (см. docker-compose.yml)
* `database_name` - имя базы данных (по-умолчанию `bpium_database`)
* `/path/bpium_dump` - указывает, из какого дампа будет восстановлена база

#### Миграция:

Достаточно переместить директорию с базой и запустить контейнер с базой.\
К примеру, нам нужно переместить на другой диск:\
`mv /home/bpium_data/postgres /mnt/sdb1/`
{% endtab %}
{% endtabs %}
