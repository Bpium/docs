# Обновление

Обновления Bpium поставляются в качестве новой сборки. Обновление включает новую сборку и файлы обновления на базу данных. В процессе обновления база данных может изменить структуру хранения данных, поэтому после обновления откат на предыдущую версию не возможен.

Чтобы сохранить возможность отката на предыдущую версию рекомендуем сделать бэкап версии и данных перед обновлением.

## **Порядок обновления**

1. **Завершите работу** сервера Bpium, Bpium S3, Bpium BPM (остановите службы/демоны)
2. **Сделайте бэкап** согласно разделу «[Бэкап и восстановление системы](https://docs.bpium.ru/deployment/extra/obsuzhivanie/bekap-i-vosstanovlenie-bazy)»
3. **Удалите службы** сервера Bpium и Bpium S3 согласно разделу 9 (только для Windows)
4. **Замените файлы** на сервере/серверах в папке Bpium файлами новой сборки\
   &#xNAN;_&#x41D;е забудьте сохранить конфигурационный файл_ `config.env.`
5. **Установите службу** Bpium S3 согласно разделу 4
6. **Установите службу** Bpium BPM согласно разделу 4
7. **Запустите сервер** Bpium S3 (запустите службы/демоны)\
   &#xNAN;_&#x442;ребуется для обновления с версии ниже 0.7 на версию 0.7.x и старше._
8. **Запустите приложение** **bpium-setup**, оно установит обновления на базу данных\
   После обновления базы данных, установщик пропишет код новой версии в системе:\
   старая версия приложения Bpium перестанет работать, новая версия — сможет.
9. **Установите службу** Bpium согласно разделу 4
10. **Запустите сервер** Bpium (запустите службы/демоны)
11. **Запустите сервер** Bpium BPM (запустите службы/демоны)

**Памятка:**

**Порядок остановки:** Bpium BPM → Bpium → Bpium S3\
Так как сервер процессов может исполнять процессы, которые обращаются к API Бипиума.

**Порядок запуска:** Bpium S3 → Bpium-setup → Bpium → Bpium BPM\
В обратном порядке. Запущенное хранилище нужно, чтобы обновление могло задействовать файлы.\
