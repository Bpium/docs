---
order: 10
title: Конвертация DOCX в PDF
---

Начиная с BPM 3.1.9 компонент процесса «DOCX -> PDF» конвертирует файл через LibreOffice. LibreOffice нужен на сервере, где запущена служба Бипиум BPM.

По умолчанию одновременно конвертируются два файла, параметр редактируемый. Третий ждёт свободное место до 60 секунд и, если его нет, шаг процесса завершается ошибкой. Служба BPM при этом продолжает работать.

## **Linux**

Установите LibreOffice и проверьте, что его видит тот же пользователь, от которого запущена служба `bpium-bpm`:

```
sudo apt install libreoffice-writer
sudo -u bpium soffice —version
```

Если команда показывает версию, в `config.env` службы BPM ничего добавлять не нужно. Если команда не находит `soffice`, укажите полный путь:

`LIBREOFFICE_BIN=/usr/bin/soffice`

После правки перезапустите службу `bpium-bpm`.

## **Windows**

Установите LibreOffice. Служба Windows обычно не видит программы из PATH пользователя, поэтому в `config.env` службы BPM укажите путь к консольной программе [`soffice.com`](http://soffice.com):

`LIBREOFFICE_BIN=C:\Program Files\LibreOffice\program\soffice.com`

`soffice.exe` не используйте: он открывает окно. После правки перезапустите службу BPM.

## **Docker**

Контейнер `bpiumdocker/bpm` не видит LibreOffice, установленный на хосте. Рядом с ним запускается отдельный контейнер конвертации. Порт наружу не публикуется.

```
libreoffice:
container_name: libreoffice
image: gotenberg/gotenberg:8-libreoffice
restart: unless-stopped
bpm:
image: bpiumdocker/bpm
environment:
BPM_SECRET: ""
BPM_QUEUE_HOST: redis
LIBREOFFICE_URL: http://libreoffice:3000/forms/libreoffice/convert
depends_on:
- redis
- libreoffice
```

`LIBREOFFICE_BIN` в этом варианте не нужен. Если задан `LIBREOFFICE_URL`, BPM отправляет файл в этот адрес и локальный `soffice` не запускает.

## **Параметры**

Параметры читаются из `config.env` службы BPM. Параметры со значением по умолчанию можно не указывать.

| **Параметр**              | **По умолчанию** | **Описание**                                                                                                                                                |
|---------------------------|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `LIBREOFFICE_URL`         | пусто            | Адрес сервиса конвертации. Нужен в Docker. Пример: [`http://libreoffice:3000/forms/libreoffice/convert`](http://libreoffice:3000/forms/libreoffice/convert) |
| `LIBREOFFICE_BIN`         | пусто            | Полный путь к `soffice` или [`soffice.com`](http://soffice.com), если программа не видна службе BPM                                                         |
| `LIBREOFFICE_CONCURRENCY` | `2`              | Сколько конвертаций docx выполнять одновременно                                                                                                             |

Файл docx скачивается на сервер BPM. На него действует общий лимит скачивания `BPM_DOWNLOAD_DATA_LIMIT`.