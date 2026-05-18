---
title: Удаление
order: 4
---

# Удаление

## Bpium

[tabs]

[tab:Windows]

1. Запустите `bpium-server-uninstall-service.bat`, он остановит и удалит службу.

2. Удалите файлы сервера Bpium.

[/tab]

[tab:Linux]

1. Остановите демон: `supervisorctl stop bpium`

2. Удалите его конфиг: `rm /etc/supervizor/conf.d/bpium.conf`

3. Перезапустите supervisor:

   ```bash
   supervisorctl reread
   supervisorctl update
   ```

4. Удалите файлы сервера Bpium.

[/tab]

[/tabs]

## Bpium S3

[tabs]

[tab:Windows]

1. Запустите `bpium-s3-uninstall-service.bat`, он остановит и удалит службу.

2. Удалите файлы сервера Bpium.

[/tab]

[tab:Linux]

1. Остановите демон: `supervisorctl stop bpium-s3`

2. Удалите его конфиг: `rm /etc/supervizor/conf.d/bpium-s3.conf`

3. Перезапустите supervisor:

   ```bash
   supervisorctl reread
   supervisorctl update
   ```

4. Удалите файлы сервера Bpium S3.

[/tab]

[/tabs]

## Bpium BPM

[tabs]

[tab:Windows]

1. Запустите `bpium-bpm-uninstall-service.bat`, он остановит и удалит службу.

2. Удалите файлы сервера Bpium.

[/tab]

[tab:Linux]

1. Остановите демон: `supervisorctl stop bpium-bpm`

2. Удалите его конфиг: `rm /etc/supervizor/conf.d/bpium-bpm.conf`

3. Перезапустите supervisor:

   ```bash
   supervisorctl reread
   supervisorctl update
   ```

4. Удалите файлы сервера Bpium BPM.

[/tab]

[/tabs]