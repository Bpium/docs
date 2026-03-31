# Действия

Действия — это основные рабочие кирпичики сценария автоматизации. Они выполняют конкретную операцию: вычисляют значение, отправляют запрос к внешнему сервису, обрабатывают данные или запускают другой сценарий.

## **Когда используются**

Действия нужны везде, где сценарий должен что-то сделать, а не просто ждать события или ветвиться. Любой сценарий состоит из цепочки действий: получили данные → обработали → отправили куда-то → записали результат.

## **Какие бывают действия**

<table data-header-hidden><thead><tr><th width="243"></th><th></th></tr></thead><tbody><tr><td>Действие</td><td>Назначение</td></tr><tr><td><a href="setvariables.md"><strong>Назначение переменных</strong></a></td><td>Сохранить значения в переменные на определённом этапе</td></tr><tr><td><a href="code.md"><strong>Код (JavaScript)</strong></a></td><td>Исполнить фрагмент кода на JS с доступом к Lodash и Moment.js</td></tr><tr><td><a href="webrequest.md"><strong>Веб-запрос</strong></a></td><td>Отправить HTTP-запрос к внешней системе</td></tr><tr><td><a href="sql.md">SQL-запрос</a></td><td>Выполнить SQL-запрос к внешней базе данных (PostgreSQL, MySQL, Oracle, SQLite, MsSQL)</td></tr><tr><td><a href="ii-zapros.md"><strong>ИИ-запрос</strong></a></td><td>Обратиться к языковой модели (YandexGpt, GigaChat, Deepseek и др.)</td></tr><tr><td><a href="konverter.md"><strong>Конвертер</strong></a></td><td>Преобразовать данные из одного формата в другой</td></tr><tr><td><a href="parse.md"><strong>Парсер</strong></a></td><td>Найти и извлечь фрагмент из текста, JSON, XML или HTML</td></tr><tr><td><a href="zapusk-processa.md"><strong>Запуск процесса</strong></a></td><td>Вызвать другой сценарий синхронно или асинхронно</td></tr></tbody></table>

