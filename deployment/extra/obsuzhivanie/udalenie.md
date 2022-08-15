# Удаление

## Bpium

{% tabs %}
{% tab title="Windows" %}
1. Запустите `bpium-server-uninstall-service.bat`, он остановит и удалит службу
2. Удалите файлы сервера Bpium
{% endtab %}

{% tab title="Linux" %}
1. Остановите демон`supervisorctl stop bpium`
2. Удалите его конфиг`rm /etc/supervizor/conf.d/bpium.conf`
3.  Перезапустите supervisor:

    ```
    supervisorctl reread
    supervisorctl update
    ```
4. Удалите файлы сервера Bpium
{% endtab %}
{% endtabs %}

## Bpium S3

{% tabs %}
{% tab title="Windows" %}
1. Запустите`bpium-s3-uninstall-service.bat`,он остановит и удалит службу
2. Удалите файлы сервера Bpium
{% endtab %}

{% tab title="Linux" %}
1. Остановите демон`supervisorctl stop bpium-s3`
2. Удалите его конфиг`rm /etc/supervizor/conf.d/bpium-s3.conf`
3.  Перезапустите supervisor:

    ```
    supervisorctl reread
    supervisorctl update
    ```
4. Удалите файлы сервера Bpium S3
{% endtab %}
{% endtabs %}

## Bpium BPM

{% tabs %}
{% tab title="Windows" %}
1. Запустите`bpium-bpm-uninstall-service.bat`, он остановит и удалит службу
2. Удалите файлы сервера Bpium
{% endtab %}

{% tab title="Linux" %}


1. Остановите демон`supervisorctl stop bpium-bpm`
2. Удалите его конфиг`rm /etc/supervizor/conf.d/bpium-bpm.conf`
3.  Перезапустите supervisor:

    ```
    supervisorctl reread
    supervisorctl update
    ```
4. Удалите файлы сервера Bpium BPM
{% endtab %}
{% endtabs %}
