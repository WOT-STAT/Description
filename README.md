# Список микросервисов WOT-STAT

## Описание

| Сервис            |  Описание |
| ----------------- | :-------- | 
| AnalyticsFrontend | Сайт отображает статистику [wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) |
| AnalyticsBackend  | Обрабатывает запросы с сайта и достаёт из БД нужные данные |
| WOTMOD            | Мод для WOT собирающий игровые события |
| EventSaver        | Сохраняет события из мода в БД |  
| ModNotification   | Служит для поддержки автообновления мода и возможных уведомлений в моде |
| SQL               | Различные SQL запросы для инициализации БД |

## CI/CD
GitHub Actions во всех сервисах настроен так:
1. Состояние ветки **master** соответствует [**dev**.wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) поддомену.
2. Релизная версия [wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) будет соответствовать релизам на гитхабе.

## Стек технологий
| Технология        |  Где используется                  |  Как используется |
|-|-|-|
| VueJS v3          | AnalyticsFrontend                  | Фреймворк на котором основан сайт |
| NodeJS            | AnalyticsBackend, EventSaver, ModNotification | Основа бэкендов |
| ClickHouse        | AnalyticsBackend, EventSaver       | Колоночная СУБД для сохранения событий |
| Clickhouse Bulk   | EventSaver                         | Прокси сервер для СУБД для групповой вставки |
| Redis             | EventSaver                         | Кеш для сохранения пар `id_боя:token`, хранится 1 час, на случай перезахода в игру |
| MongoDB           | AnalyticsBackend                   | Сохраняет пермалинки на состояние бэкенда |
