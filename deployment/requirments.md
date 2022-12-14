# 🖥 Требования

## Для сервера приложения Bpium

Bpium для работы использует одно ядро процессора (в будущем будет несколько), поэтому важна не многоядерность, а частота процессора. Для эффективной работы Бипиума и операционной системы суммарно нужно не менее 2 ядер процессора.

#### Минимальные требования к серверу:

* Процессор: 2 ГГц (рекомендуется Intel i5/i7, 3+ ГГц)
* Память: 2 Гб (рекомендуется от 4 Gb)
* Диск: 10 Гб свободного места
* Локальная сеть: 100 Мб/c
* Доступ в интернет: периодически для активации лицензий
* Операционная система: Windows, Mac OS X
* Для работы модуля телефонии: внешний IP-адрес, домен, сертификат HTTPS

## **Для сервера баз данных PostgreSQL**

Bpium для хранения данных использует базу данных PostgreSQL. PostgreSQL многоядерный процесс.

#### **Минимальные требования к серверу:**

* Процессор: 2 ГГц (рекомендуется Intel i5/i7, 4 ядра или больше, 3+ ГГц)
* Память: 2 Гб (рекомендуется 8 Gb)
* Диск: 10 Гб свободного места
* Локальная сеть: 100 Мб/c
* Доступ в интернет: опционально
* Операционная система: Windows, Linux, Mac OS X

_Сервер баз данных может быть совмещен с сервером приложения._

## **Для сервера файлового хранилища Bpium S3**

Bpium для хранения файлов может использовать облако Amazon S3 или свой сервер Bpium S3.

**Минимальные требования к серверу:**

* Процессор: 2 ГГц (рекомендуется Intel i3/i5, 2 ядра или больше)
* Память: 2 Гб (рекомендуется 4 Gb)
* Диск: 10 Гб + XXX Гб свободного места для хранения файлов
* Локальная сеть: 100 Мб/c
* Доступ в интернет: опционально
* Операционная система: Windows, Linux, Mac OS X
* Для работы с сервером Bpium c HTTPS: внешний IP-адрес, домен, сертификат HTTPS

_Сервер файлового хранилища может быть совмещен с сервером приложения._

## **Для сервера бизнес процессов Bpium BPM**

Bpium для исполнения сценариев использует собственный сервер приложений Bpium BPM.

**Минимальные требования к серверу:**

* Процессор: 3 ГГц (рекомендуется Intel i5/i7, 4 ядра или больше)
* Память: 8 Гб
* Диск: 20 Гб
* Локальная сеть: 100 Мб/c
* Доступ в интернет: опционально
* Операционная система: Windows, Linux, Mac OS X

_Сервер бизнес процессов может быть совмещен с сервером приложения._

## **Для очереди сервера бизнес процессов Redis**

Сервера бизнес процессов используют единую очередь и общее хранилище данных на базе Redis.

**Минимальные требования к серверу:**

* Процессор: 1 ГГц
* Память: 2 Гб
* Диск: 10 Гб
* Локальная сеть: 100 Мб/c
* Доступ в интернет: опционально
* Операционная система: Windows, Linux, Mac OS X

_Сервер очереди может быть совмещен с сервером бизнес процессов_

## Рабочие места сотрудников

Bpium — веб-приложение, работает в браузере.

**Минимальные требования к серверу:**

* Процессор: 2 ГГц (рекомендуется Intel i3/i5, 4 ядра)
* Память: 4 Гб (рекомендуется 8 Гб)
* Диск: 10 Гб свободного места
* Локальная сеть: 100 Мб/c
* Доступ в интернет: опционально
* Операционная система: Windows, Linux, Mac OS
* Браузер: Google Chrome последней версии
