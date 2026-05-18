---
icon: square-ring
title: На одном сервере (Службы)
order: 0.9
---

## **Состав дистрибутива**

Bpium/Bpium S3/Bpium BPM - распространяется как исполняемый файл в разных форматах для разных операционных систем (Windows, Linux, Mac OS X). Приложение само является веб-сервером для себя. Дополнительные веб-сервера не требуются, но могут использоваться для проксирования запросов или как балансировщики в схемах с несколькими серверами.

[tabs]

[tab:Bpium]

-  `bpium.exe` -- сервер приложения

-  `bpium-setup.exe` -- скрипт развертывания/обновления структуры базы данных

-  `config-example.env` -- пример файла с настройками работы сервера

-  `bpium-server-install-service.bat` -- скрипт для создания службы Windows

-  `bpium-server-uninstall-service.bat` -- скрипт для удаления службы Windows

-  `nssm.exe` -- вспомогательная программа для создания службы Windows

**Linux:**

-  `bpium` -- сервер приложения

-  `bpium-setup` -- скрипт развертывания/обновления структуры базы данных

-  `config-example.env` -- пример файла с настройками работы сервера

[/tab]

[tab:Bpium S3]

**Windows:**

-  `bpium-s3.exe` -- сервер локального файлового хранилища

-  `config-example.env` -- пример файла с настройками работы сервера

-  `bpium-s3-install-service.bat` -- скрипт для создания службы Windows

-  `bpium-s3-uninstall-service.bat` -- скрипт для удаления службы Windows

-  `nssm.exe` -- программа для создания службы Windows

**Linux:**

-  `bpium-s3` -- сервер локального файлового хранилища

-  `config-example.env` -- пример файла с настройками работы сервера

[/tab]

[tab:Bpium BPM]

**Windows:**

-  `bpium-bpm.exe` -- сервер исполнения бизнес-процессов

-  `config-example.env` -- пример файла с настройками работы сервера

-  `bpium-bpm-install-service.bat` -- скрипт для создания службы Windows

-  `bpium-bpm-uninstall-service.bat` -- скрипт для удаления службы Windows

-  `nssm.exe` -- программа для создания службы Windows

**Linux:**

-  `bpium-bpm` -- сервер исполнения бизнес-процессов

-  `config-example.env` -- пример файла с настройками работы сервера

[/tab]

[/tabs]

[tab]

[tab]

## Подготовка сервера

### **Доменное имя**

Bpium может работать как по IP-адресу, так и по доменному имени. Настройка домена для Bpium не имеет индивидуальных особенностей, поэтому не входит в эту инструкцию.

### **SSL-сертификаты**

[tabs]

[tab:Bpium]

**Сервер приложения Bpium**

Bpium может работать на защищенном канале связи (HTTPS). Для этого сервер приложения должен быть доступен из интернета, иметь доменное имя и SSL-сертификат Class 1 или более высокий.\
Для работы мультидоменной версии Bpium требуется wildcard-сертификат Class 2 или более высокий.

[/tab]

[tab:Bpium S3]

**Локальное хранилище Bpium S3**

Локальный сервер хранилища Bpium S3 также может работать на защищенном канале связи (HTTPS).  Для этого он также должен быть доступен из интернета, иметь доменное имя и SSL-сертификат Class 1 или более высокий.

Не используйте самоподписные сертификаты, они не будут приняты сервером. Для создания сертификата мы рекомендуем сервис [letsencrypt.org](http://letsencrypt.org) (сертификаты Class 1 -- бесплатные).

[/tab]

[/tabs]

[tab]

[tab]

### **Фаервол**

[tabs]

[tab:Bpium]

**Для сервера c приложением Bpium**

Входящие подключения к серверу:

-  по порту, на котором работает Bpium (по умолчанию: 80 для HTTP, 443 для HTTPS)

Исходящие подключения от сервера:

-  на адрес сервера баз данных (по умолчанию 5432)

-  на адрес сервера с приложением Bpium BPM (порт по умолчанию: 2030)

-  на адрес сервера с локальным хранилищем Bpium S3 (порт по умолчанию: 2020)

-  на адрес сервера с внешним хранилищем S3, если используется внешнее

-  на адрес \*.bpium.ru по портам 80 и 443 (для системы лицензирования и обновления)

-  если вы используете [вебхуки](./../../bpium-setup/systemcatalogs/upravlenie/webhooks), то на их адреса и порты

[/tab]

[tab:Bpium BPM]

Для сервера c приложением Bpium BPM.

**Входящие подключения к серверу:**

-  `Bpium → Bpium BPM` \
   с адреса сервера с приложением Bpium по порту, на котором работает Bpium BPM (по умолчанию 2020)

**Исходящие подключения от сервера:**

-  `Bpium BPM → Bpium` \
   на адрес сервера с приложением Bpium (порт по умолчанию: 80  / 443 для HTTPS)

-  `Bpium BPM → Bpium S3` \
   на адрес сервера с локальным хранилищем Bpium S3 (порт по умолчанию: 2020)

-  `Bpium BPM → внешний S3` \
   на адрес сервера с внешним хранилищем S3, если используется внешнее

-  `Bpium BPM → внешний мир` \
   если в сценарии вы используете компоненты [Веб-запрос](./../../bpium-setup/processes/components/deistviya/webrequest), то на адреса и порты, используемые в запросах

-  `Bpium BPM → внешний мир` \
   если в сценарии вы используете компоненты [SQL-запрос](./../../bpium-setup/processes/components/deistviya/sql), то на адреса и порты, используемые в запросах

[/tab]

[tab:Bpium S3]

Для сервера c локальным файловым хранилищем Bpium S3.

**Входящие подключения к серверу:**

-  со всех внешних и внутренних адресов по порту, на котором работает Bpium S3 (по умолчанию 2020)

**Исходящие подключения от сервера:**

-  на все адреса по установленным соединениям

[/tab]

[tab:PostgreSQL]

Для сервера c базой данных Postgre SQL.

**Входящие подключения к серверу:**

-  `Bpium → Postgre SQL`\
   с адреса сервера с приложением Bpium по порту, на котором работает Postgre SQL (по умолчанию 5432)

-  `Bpium BPM → Postgre SQL`

   если в сценарии вы используете компоненты SQL-запрос, для подключения к базе данных, то с адреса сервера с приложением Bpium BPM

**Исходящие подключения от сервера:**

-  на все адреса по установленным соединениям

[/tab]

[tab:Redis]

Для сервера с операционным хранилищем Redis.

**Входящие подключения к серверу:**

-  `Bpium BPM → Redis` \
   с адреса сервера с приложением Bpium BPM на порт Redis (по умолчанию 6379)

**Исходящие подключения от сервера:**

-  на все адреса по установленным соединениям

[/tab]

[/tabs]

[tab]

[tab]

[tab]

[tab]

[tab]



## Установка и настройка зависимостей

### **Установка PostgreSQL**

Bpium для хранения использует базу данных PostgreSQL.

#### Требования:

-  Версия не ранее 9.4 (желательно не ранее 9.6, допустимы 10+, 11+, 12+, 13+, 14+ )

:::info 

Версии PostgreSQL 15+ на текущий момент не поддерживаются.

:::

-  Сервер PostgreSQL установлен на том же компьютере, что и Bpium, или на компьютере в пределах локальной сети (если на удаленном сервере, то будут большие задержки)

-  Сервер PostgreSQL должен работать как служба (в Windows)

-  Требуется самостоятельная настройка резервирования и бэкапирования базы данных

Скриншоты процесса стандартной установки PostgreSQL версии 15.2 представлены ниже:

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 143624.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 143700.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 143718.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 143734.png>)

<figcaption>



</figcaption>

</figure>

В следующем шаге  установки необходимо задать пароль для суперюзера (postgres).\
**Важно!** Запомните данный пароль, он понадобится в будущем для работы с базами данных.

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 143935.png>)

<figcaption>



</figcaption>

</figure>

В следующих шагах нужно указать порт (по умолчанию: 5432) и выбрать локаль.\
**Важно!** Необходимо выбрать локаль "Russian, Russia" (`--locale=ru_RU)`, иначе не будет поддержки кириллицы.

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 144003.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 144029.png>)

<figcaption>



</figcaption>

</figure>

В следующих шагах нужно просто нажимать Next и дождаться окончания установки.

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 144100.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 144118.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 144148.png>)

<figcaption>



</figcaption>

</figure>

Когда установка завершится нажмите кнопку "Finish"

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 144543.png>)

<figcaption>



</figcaption>

</figure>

Также для старта системы понадобится заранее созданная пустая база данных. Для её создания запустите программу pgAdmin (она устанавливается совместно с PostgreSQL). Создать пустую базу данных можно по инструкции из скриншотов ниже.

При первом запуске pgAdmin необходимо создать мастер-пароль. Придумайте и введите пароль, нажмите "ОК".\
**Важно!** Запомните данный пароль, так как он будет запрашиваться в дальнейшем при каждом входе в pgAdmin.

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 172405.png>)

<figcaption>



</figcaption>

</figure>

В следующем шаге будет запрошен пароль для подключения к серверу базы данных. Необходимо ввести пароль, который был указан при установке PostgrSQL

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 172802.png>)

<figcaption>



</figcaption>

</figure>

Далее создаем пустую базу данных:

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145620.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145651.png>)

<figcaption>



</figcaption>

</figure>

Готово! Пустая база создана. Вместо пустой базы данных может использоваться скопированная из облака база данных компании.

### **Копирование данных из базы в облаке**

При первом запуске Бипиум создает в базе служебные данные: раздел «Система» и служебные каталоги. Если вместо пустой базы использовать базу с данными из облачной версии Бипиума, то Бипиум при первом запуске создаст служебные данные в ней. В ней будут и служебные данные для работы коробочной версии и пользовательские данные, созданные в облаке. \
Подробнее:  [Перенос базы из облака](./../nastroika-i-zapusk/perenos-bazy-iz-oblaka)

### **Установка Redis**

Bpium BPM для хранения данных и распределения нагрузки использует хранилище Redis.

**Требования:**

-  Версия: последняя

-  Redis должен работать как служба (в Windows)

-  Требуется самостоятельная настройка параметров аутентификации (опционально)

Ссылка сборки для Windows: \
[**https://github.com/MicrosoftArchive/redis/releases**](https://github.com/MicrosoftArchive/redis/releases)

Для стандартной установки Redis скачайте msi-пакет по ссылке выше и запустите его. Шаги установки указаны ниже:

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145049.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145105.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145124.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145136.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145151.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145208.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145226.png>)

<figcaption>



</figcaption>

</figure>

После окончания установки проверьте, что Redis в службах запущен и установлен Автоматический тип запуска. Для этого нажмите Win+R, наберите services.msc и нажмите ОК. В списке служб найдите Redis и проверьте состояние.

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145250.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 145338.png>)

<figcaption>



</figcaption>

</figure>

## **Установка Bpium**

Распакуйте архив

-  **Файл должен лежать в папке и подпапках без русских символов**

-  Пользователю операционной системы, от имени которого запускается приложение, должны быть даны права создавать файлы в папке Bpium

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 150330.png>)

<figcaption>



</figcaption>

</figure>



-  Настройте конфигурационный файл config.env. Пример файла config.env с минимально необходимыми параметрами для запуска системы указан ниже (в данном примере config.env файл единый для всех трех приложений системы).\
   [Описание параметров](./../nastroika-i-zapusk/parametry-config.env/bpium).

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 151007.png>)

<figcaption>



</figcaption>

</figure>

-  Запустите файл `bpium-setup.exe` (под Windows) и `bpium-setup` (под Linux), он создаст в базе данных нужную структуру

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 151158.png>)

<figcaption>



</figcaption>

</figure>



1. Зарегистрируйте bpium как службу/демон, чтобы он запускался при старте системы.

[tabs]

[tab:Windows]

1. Запустите файл `bpium-server-install-service.bat`, он зарегистрирует Bpium как службу Windows и запустит её. Имя службы: Bpium Server.

:::danger 

Файл **bpium-server-install-service.bat** запустить от имени администратора (правой кнопкой мыши по имени файла и выбрать «Запустить от имени администратора»).

:::

[/tab]

[tab:Linux (через supervisor)]

1. Вы можете использовать любой пакет для обертывания приложения в демон. Мы рекомендуем использовать supervisor. Установка пакета:

   ```
   sudo apt-get install supervisor -y
   sudo /etc/init.d/supervisor restart
   ```

2. В папке с Bpium создайте файл скрипта командой touch bpium-start.sh

3. В любом удобном редакторе напишите в файле скрипт:

   ```
   cd /path/to/bpium/folder
   ./bpium
   ```

4. Дайте файлу права на исполнение:

   ```
   sudo chmod +x ./bpium-start.sh
   ```

5. Создайте конфигурационный файл демона `«/etc/supervisor/conf.d/bpium.conf»`:

   ```
   [program:bpium]
   command=/path/to/bpium-start.sh
   stdout_logfile=/var/log/bpium-server-out.log
   stderr_logfile=/var/log/bpium-server-error.log
   autostart=true
   autorestart=true
   startsecs=10
   numprocs=1
   ```

6. Примените установленные настройки supervisor:

   ```
   supervisorctl reread
   supervisorctl update
   ```

7. Запустите службу:

   ```
   supervisorctl start bpium
   ```

[/tab]

[tab:Linux (через systemd)]

1. В папке с Bpium создайте файл скрипта командой

   ```
   touch bpium-start.sh
   ```

2. В любом удобном редакторе напишите в файле скрипт:

   ```
    #!/bin/bash
    cd /path/to/bpium
    ./bpium
   ```

3. Дайте файлу права на исполнение:

   ```
   sudo chmod +x bpium-start.sh
   ```

4. Создайте конфигурационный файл демона«/etc/systemd/system/bpium.service»:

   ```<code>[Unit]
   Description=bpium-start
   After=multi-user.target
   After=bpium-bpm.service
   Requires=bpium-bpm.service
   [Service]
   Type=idle
   WorkingDirectory=/path/to/bpium
   ExecStart=/path/to/bpium/bpium-start.sh
   [Install]
   WantedBy=multi-user.target
   ```

5. Перезапустите демон systemd:

   ```
   systemctl daemon-reload
   ```

6. И включите службу:

   ```<code><strong>systemctl enable bpium.service
   systemctl enable bpium.service
   ```

[/tab]

[/tabs]

[tab]



[tab]

[tab]

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 151158 (1).png>)

<figcaption>



</figcaption>

</figure>

После установки проверьте, что служба Bpium запущена и Тип запуска "Автоматически".

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 151806.png>)

<figcaption>



</figcaption>

</figure>

## **Установка Bpium S3**

Хранилище, в процессе работы, может заниматься много места, поэтому лучше всего выносить его на другой диск или СХД. Для удобства рекомендуется сделать символическую ссылку на директорию, где будет находится хранилище.

[tabs]

[tab:Windows]

`mklink /D назначение цель`\
`/D` – указывает, что ссылка будет на директорию\
`назначение` – место, где будет размещена ссылка, пример: `c:\bpium\storage`\
`цель` – директория на которую ссылается ссылка, пример: `d:\s`

[/tab]

[tab:Linux]

`ln - s назначение цель`\
`-s` - указывает, что создаем символическую ссылку\
*`назначение`*– место, где будет размещена ссылка, пример: `/opt/bpium/storage`\
`цель` – директория на которую ссылается ссылка, пример: `/mnt/volume1/s3/`

[/tab]

[/tabs]

[tab]

[tab]



1. Распакуйте архив

   -  **Файл должен лежать в папке и подпапках без русских символов**

   -  Пользователю, от имени которого запускается приложение, должны быть даны права создавать файлы в папке Bpium

2. Настройте конфигурационный файл `config.env`\
   [Описание параметров](./../nastroika-i-zapusk/parametry-config.env/bpium-s3).

3. Зарегистрируйте bpium-s3 как службу/демон, чтобы он запускался при старте системы

[tabs]

[tab:Windows]

1. Запустите файл `bpium-s3-install-service.bat`, он зарегистрирует Bpium S3 как службу Windows и запустит её. Имя службы: bpium-s3.

:::danger 

Файл **bpium-s3-install-service.bat** запустить от имени администратора (правой кнопкой мыши по имени файла и выбрать «Запустить от имени администратора»).

:::

[/tab]

[tab:Linux (через supervisor)]

1. Установка пакета:

   ```
   sudo apt-get install supervisor
   sudo /etc/init.d/supervisor restart
   ```

2. В папке с Bpium создайте файл скрипта командой `touch bpium-s3-start.sh`

3. В любом удобном редакторе напишите в файле скрипт:

   ```
   #!/bin/bash
    cd /path/to/bpium/folder
    ./bpium-s3
   ```

4. Дайте файлу права на исполнение:

   ```
   sudo chmod +x ./bpium-s3-start.sh
   ```

5. Создайте конфигурационный файл демона `«/etc/supervisor/conf.d/bpium-s3.conf»`:

   ```
   [program:bpium-s3]
   command=/path/to/bpium-s3
   stdout_logfile=/var/log/bpium-s3-out.log
   stderr_logfile=/var/log/bpium-s3-error.log
   autostart=true
   autorestart=true
   startsecs=10
   numprocs=1
   ```

6. Примените установленные настройки supervisor:

   ```
   supervisorctl reread
   supervisorctl update
   ```

7. Запустите службу:

   ```
   supervisorctl start bpium-s3
   ```

[/tab]

[tab:Linux (через systemd)]

1. В папке с Bpium создайте файл скрипта командой `touch bpium-start.sh`

2. В любом удобном редакторе напишите в файле скрипт:

   ```
   #!/bin/bash
    cd /path/to/bpium
    ./bpium-s3
   ```

3. Дайте файлу права на исполнение:

   ```
   sudo chmod +x bpium-s3-start.sh
   ```

4. Создайте конфигурационный файл демона`«/etc/systemd/system/bpium-s3.service»`:

   ```
   [Unit]
   Description=bpium-s3-start
   After=multi-user.target
   After=redis-server.service
   Requires=redis-server.service
   [Service]
   Type=idle
   WorkingDirectory=/opt/bpium1.2.0
   ExecStart=/path/to/bpium/bpium-s3-start.sh
   [Install]
   WantedBy=multi-user.target
   ```

5. Перезапустите демон systemd:

   ```
   systemctl daemon-reload
   ```

6. И включите службу:

   ```
   systemctl enable bpium-s3.service
   ```

[/tab]

[/tabs]

[tab]

[tab]

[tab]

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 151859.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 151927.png>)

<figcaption>



</figcaption>

</figure>

## Установка Bpium BPM

1. Установите Redis

2. Распакуйте архив

   -  **Файл должен лежать в папке и подпапках без русских символов**

   -  Пользователю, от имени которого запускается приложение, должны быть даны права создавать файлы в папке Bpium

3. Настройте конфигурационный файл `config.env`\
   [Описание параметров](./../nastroika-i-zapusk/parametry-config.env/bpium-bpm).

4. Зарегистрируйте bpium-bpm как службу/демон, чтобы он запускался при старте системы

[tabs]

[tab:Windows]

1. Запустите файл `bpium-bpm-install-service.bat,` он зарегистрирует Bpium BPM как службу Windows и запустит её. Имя службы: bpium-bpm.

:::danger 

Файл **bpium-bpm-install-service.bat** запустить от имени администратора (правой кнопкой мыши по имени файла и выбрать «Запустить от имени администратора»)

:::

[/tab]

[tab:Linux (через supervisor)]

1. Установка пакета:

   ```
   sudo apt-get install supervisor
   sudo /etc/init.d/supervisor restart
   ```

2. В папке с Bpium создайте файл скрипта командой `touch bpium-bpm-start.sh`

3. В любом удобном редакторе напишите в файле скрипт:

   ```
   #!/bin/bash
   cd /path/to/bpium/folder
   ./bpium-bpm
   ```

4. Дайте файлу права на исполнение:

   ```
   sudo chmod +x ./bpium-bpm-start.sh
   ```

5. Создайте конфигурационный файл демона `«/etc/supervizor/conf.d/bpium-bpm.conf»`:

   ```
   [program:bpium-bpm]
   command=/path/to/bpium-bpm
   stdout_logfile=/var/log/bpium-bpm-out.log
   stderr_logfile=/var/log/bpium-bpm-error.log
   autostart=true
   autorestart=true
   startsecs=10
   numprocs=1
   ```

6. Примените установленные настройки supervisor:

   ```
   supervisorctl reread
   supervisorctl update
   ```

7. Запустите службу:

   ```
   supervisorctl start bpium bpm
   ```

[/tab]

[tab:Linux (через systemd)]

1. В папке с Bpium создайте файл скрипта командой `touch bpium-bpm-start.sh`

2. В любом удобном редакторе напишите в файле скрипт:

   ```
   #!/bin/bash
   cd /path/to/bpium
   ./bpium-bpm
   ```

3. Дайте файлу права на исполнение:

   ```
   sudo chmod +x bpium-bpm-start.sh
   ```

4. Создайте конфигурационный файл демона`«/etc/systemd/system/bpium-bpm.service»`:

   ```
   [Unit]
   Description=bpium-bpm-start
   After=multi-user.target
   After=bpium-s3.service
   Requires=bpium-s3.service
   [Service]
   Type=idle
   WorkingDirectory=/path/to/bpium
   ExecStart=/path/to/bpium/bpium-bpm-start.sh
   [Install]
   WantedBy=multi-user.target
   ```

5. Перезапустите демон systemd:

   ```
   systemctl daemon-reload
   ```

6. И включите службу:

   ```
   systemctl enable bpium-bpm.service
   ```

[/tab]

[/tabs]

[tab]

[tab]

[tab]

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 152000.png>)

<figcaption>



</figcaption>

</figure>

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 152059.png>)

<figcaption>



</figcaption>

</figure>

## Проверка системы

Установка завершена, можно попробовать зайти в Bpium. Для этого откройте браузер и в адресной строке наберите адрес хоста, который указали для Bpium-Server'а в файле config.env

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 152153.png>)

<figcaption>



</figcaption>

</figure>

Стандартный логин и пароль для входа admin admin, наберите их и нажмите Войти. При успешном входе увидете следующее окно:

<figure>

![](<../../.gitbook/assets/Снимок экрана 2023-03-23 152238.png>)

<figcaption>



</figcaption>

</figure>

Bpium работает. Теперь можете создавать свою систему.

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]

[/tab]