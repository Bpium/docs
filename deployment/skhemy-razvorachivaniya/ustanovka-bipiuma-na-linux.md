---
order: 0.7
title: Установка на Linux
---

В этой статье описана установка Бипиума на Linux-сервер.

Компоненты Бипиума запускаются как службы `systemd`.

В результате на сервере будут работать:

-  **Bpium** -- сервер приложения;

-  **Bpium BPM** -- сервер исполнения процессов;

-  **Bpium S3** -- локальное файловое хранилище;

-  **PostgreSQL** -- база данных;

-  **Redis** -- хранилище задач и состояния BPM.

:::quote 

PostgreSQL, Redis и файловое хранилище могут быть размещены на других серверах. В этом случае укажите их адреса в конфигурации Бипиума.

:::

## Перед установкой

Ознакомьтесь с:

-  [системными требованиями](./../sistemnye-trebovaniya)

-  [архитектурой и компонентами](./../architecture)

-  [параметрами config.env](./../nastroika-i-zapusk/parametry-config.env/_index)

Для установки потребуются права `root` или возможность выполнять команды через `sudo`.

## Сетевые порты

По умолчанию используются следующие порты:

| Компонент  | Порт     |
|------------|----------|
| Bpium      | 80 / 443 |
| Bpium BPM  | 2030     |
| Bpium S3   | 2020     |
| PostgreSQL | 5432     |
| Redis      | 6379     |

Если все компоненты установлены на одном сервере, PostgreSQL, Redis и BPM не требуется публиковать во внешний интернет.

Открывайте наружу только те порты, которые необходимы для выбранной схемы развертывания.

## Установите PostgreSQL и Redis

Для работы Бипиума требуется:

-  PostgreSQL версии 17;

-  Redis версии 6 или выше.

PostgreSQL и Redis можно установить на этом же сервере или использовать уже существующие экземпляры в локальной сети.

### Пример для Debian / Ubuntu

```bash
sudo apt update
sudo apt install postgresql redis-server
```

Проверьте состояние служб:

```bash
systemctl status postgresql
systemctl status redis-server
```

Проверьте Redis:

```bash
redis-cli ping
```

Ожидаемый ответ:

```text
PONG
```

### Создайте базу данных

Создайте отдельного пользователя и базу данных PostgreSQL для Бипиума.

Пример:

```sql
CREATE USER bpium WITH PASSWORD 'strong_password';
CREATE DATABASE bpium OWNER bpium;
```

Проверьте подключение:

```bash
psql -h 127.0.0.1 -U bpium -d bpium
```

:::quote 

Для промышленной эксплуатации настройте регулярное резервное копирование PostgreSQL.

:::

## Настройка Бипиума

### Подготовьте каталог Бипиума

Создайте каталог для установки:

```bash
sudo mkdir -p /opt/bpium
```

Рекомендуется использовать отдельного системного пользователя:

```bash
sudo useradd \
  --system \
  --home /opt/bpium \
  --shell /usr/sbin/nologin \
  bpium
```

### Скопируйте дистрибутив

[Скачайте](./../../changelog/_index) и разархивируйте файлы дистрибутива Бипиума в:

```text
/opt/bpium
```

В каталоге должны находиться файлы:

```text
/opt/bpium/
├── bpium
├── bpium-assets
├── bpium-setup
├── bpium-bpm
├── bpm-assets
├── bpium-s3
├── s3-assets
└── config-example.env
```

Фактический состав дистрибутива может отличаться в зависимости от версии.

Выдайте исполняемым файлам права на запуск:

```bash
sudo chmod +x /opt/bpium/bpium
sudo chmod +x /opt/bpium/bpium-setup
sudo chmod +x /opt/bpium/bpium-bpm
sudo chmod +x /opt/bpium/bpium-s3
```

Назначьте владельца:

```bash
sudo chown -R bpium:bpium /opt/bpium
```

### Настройте config.env

Создайте или отредактируйте:

```text
/opt/bpium/config.env
```

Укажите параметры подключения к PostgreSQL, Redis, BPM и файловому хранилищу.

Пример:

```dotenv
DB_CONNECTION_STRING=postgres://bpium:strong_password@127.0.0.1:5432/bpium

COOKIE_SECRET=replace_with_random_secret
SCRIPTS_TOKEN_SECRET=replace_with_random_secret

HOST=bpium.example.com

BPM_HOST=127.0.0.1
BPM_PORT=2030
BPM_SECRET=replace_with_random_secret

S3_LOCAL=true
S3_HOST=files.example.com
S3_PORT=2020
S3_KEY=replace_with_access_key
S3_SECRET=replace_with_secret_key

SERIAL_NUMBER=your_serial_number
```

Полный список параметров приведен в разделе [Параметры config.env](./../nastroika-i-zapusk/parametry-config.env/_index).

Ограничьте доступ к конфигурационному файлу:

```bash
sudo chown bpium:bpium /opt/bpium/config.env
sudo chmod 600 /opt/bpium/config.env
```

### Инициализируйте базу данных

Перед первым запуском выполните:

```bash
cd /opt/bpium
sudo -u bpium ./bpium-setup
```

Команда создаст или обновит необходимую структуру базы данных.

Если команда завершилась с ошибкой, не запускайте остальные компоненты до устранения причины.

### Создайте службу Bpium S3

Создайте файл:

```text
/etc/systemd/system/bpium-s3.service
```

Содержимое:

```ini
[Unit]
Description=Bpium S3
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=bpium
Group=bpium
WorkingDirectory=/opt/bpium
EnvironmentFile=/opt/bpium/config.env
ExecStart=/opt/bpium/bpium-s3
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Создайте службу Bpium BPM

Создайте файл:

```text
/etc/systemd/system/bpium-bpm.service
```

Содержимое:

```ini
[Unit]
Description=Bpium BPM
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=bpium
Group=bpium
WorkingDirectory=/opt/bpium
EnvironmentFile=/opt/bpium/config.env
ExecStart=/opt/bpium/bpium-bpm
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Создайте службу Bpium

Создайте файл:

```text
/etc/systemd/system/bpium.service
```

Содержимое:

```ini
[Unit]
Description=Bpium
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=bpium
Group=bpium
WorkingDirectory=/opt/bpium
EnvironmentFile=/opt/bpium/config.env
ExecStart=/opt/bpium/bpium
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Примените конфигурацию systemd

Выполните:

```bash
sudo systemctl daemon-reload
```

Добавьте службы в автозапуск:

```bash
sudo systemctl enable bpium-s3
sudo systemctl enable bpium-bpm
sudo systemctl enable bpium
```

Запустите их:

```bash
sudo systemctl start bpium-s3
sudo systemctl start bpium-bpm
sudo systemctl start bpium
```

### Проверьте состояние служб

Выполните:

```bash
systemctl status bpium
systemctl status bpium-bpm
systemctl status bpium-s3
```

При успешном запуске службы должны находиться в состоянии:

```text
active (running)
```

### Проверьте журналы

Последние сообщения:

```bash
journalctl -u bpium -n 100
journalctl -u bpium-bpm -n 100
journalctl -u bpium-s3 -n 100
```

Просмотр журнала в реальном времени:

```bash
journalctl -u bpium -f
```

Проверить прослушиваемые порты можно командой:

```bash
ss -lntp
```

### Настройте HTTPS

Для промышленной эксплуатации рекомендуется использовать HTTPS.

Можно:

-  настроить HTTPS непосредственно в Bpium;

-  разместить перед Bpium reverse proxy, например Nginx.

При использовании reverse proxy наружу обычно публикуется только порт `443`, а PostgreSQL, Redis и внутренние сервисы остаются недоступны из внешней сети.

Используемый сертификат должен быть доверенным для клиентов и компонентов, взаимодействующих с Бипиумом.

## Проверка работы Бипиума

Откройте в браузере адрес, указанный в `HOST`.

Например:

```text
https://bpium.example.com
```

После успешного открытия интерфейса выполните [проверку после установки](./../nastroika-i-zapusk/zapusk).

## Обновление

Перед обновлением:

1. создайте резервную копию PostgreSQL;

2. создайте резервную копию файлового хранилища;

3. сохраните текущий `config.env`;

4. сохраните предыдущую версию дистрибутива для возможного отката.

Порядок действий описан в статье [Обновление](./../obsluzhivanie/obnovlenie).

## Удаление

Остановите и отключите службы:

```bash
sudo systemctl disable --now bpium
sudo systemctl disable --now bpium-bpm
sudo systemctl disable --now bpium-s3
```

Удалите unit-файлы:

```bash
sudo rm /etc/systemd/system/bpium.service
sudo rm /etc/systemd/system/bpium-bpm.service
sudo rm /etc/systemd/system/bpium-s3.service
```

Примените изменения:

```bash
sudo systemctl daemon-reload
```

После этого можно удалить файлы приложения:

```bash
sudo rm -rf /opt/bpium
```

Не удаляйте PostgreSQL, базу данных и файловое хранилище, пока не убедитесь, что данные больше не нужны.