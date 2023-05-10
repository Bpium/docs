---
description: Инструкция описывает только ручную сертификацию через Let's Encrypt.
---

# TLS/SSL Сертификат

## Получение сертификата

Для получения сертификата нужно иметь:

* Доступ из внешней сети - "белый" IP
* Открытый 80 порт

{% tabs %}
{% tab title="Windows" %}
Автоматическую выдачу сертификатов осуществляет Crypt-LE, а в качестве веб-сервера мы используем python.

1. Скачайте программу со страницы: [https://github.com/do-know/Crypt-LE/releases](https://github.com/do-know/Crypt-LE/releases) и распакуйте в любую удобную директорию
2. Скачайте и установите python: [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)
3. Запустите вебсервер(_в той же директории, где находится le32.exe/le64.exe_) командой:`python -m http.server 80`
4. Откройте командную строку и введите команду:`le64.exe --email "ваш почтовый адрес" --domains "domain.com" --export-pfx "пароль_для_сертификата pfx" --key "account.key" --csr "domain.csr" --csr-key "domain.key" --crt "domain.crt" --generate-missing --unlink`\
   Если все успешно, вы получите тестовый сертификат
5. Для получения "боевого" вам нужно добавить к предыдущей команде параметр: `--live`
{% endtab %}

{% tab title="Ubuntu Linux" %}
Для получения сертификата будет использоваться certbot.

1. Устанавливаем certbot: `apt-get install certbot`
2. Запускаем certbot для получения тестового сертификата:\
   `certbot --standalone -m "ваш почтовый адрес" -d "domain.com" --test-cert --agree-tos`
3. Если прошлый этап завершился успешно, получаем "боевой":\
   `certbot --standalone -m "ваш почтовый адрес" -d "domain.com" --agree-tos`
{% endtab %}
{% endtabs %}
