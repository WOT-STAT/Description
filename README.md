# Описание модулей проекта WOT-STAT

## Оглавление
1. [Описание](#описание)
   1. [CI/CD](#cicd)
   2. [Стек технологий](#стек-технологий)
2. [Принцип работы](#принцип-работы)
3. [Работа сервера](serve.md)
4. [Схема БД](tables/README.md)
5. [Репозиторий со старой версией](https://github.com/SoprachevAK/wot-stat/tree/v1.0.0.0) 

***

## Описание

| Сервис                                                             | Описание                                                                               |
| ------------------------------------------------------------------ | :------------------------------------------------------------------------------------- |
| [AnalyticsFrontend](https://github.com/WOT-STAT/AnalyticsFrontend) | Сайт отображает статистику [wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) |
| [AnalyticsBackend](https://github.com/WOT-STAT/AnalyticsBackend)   | Обрабатывает запросы с сайта и достаёт из БД нужные данные                             |
| [WOTMOD](https://github.com/WOT-STAT/WOTMOD)                       | Мод для WOT собирающий игровые события                                                 |
| [EventSaver](https://github.com/WOT-STAT/EventSaver)               | Сохраняет события из мода в БД                                                         |
| [ModNotification](https://github.com/WOT-STAT/ModNotification)     | Служит для поддержки автообновления мода и возможных уведомлений в моде                |

## CI/CD
GitHub Actions во всех сервисах настроен так:
1. Состояние ветки **master** соответствует [**dev**.wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) поддомену.
2. Релизная версия [wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) будет соответствовать релизам на гитхабе.

## Стек технологий
| Технология      | Где используется                              | Как используется                                                                   |
| --------------- | --------------------------------------------- | ---------------------------------------------------------------------------------- |
| VueJS v3        | AnalyticsFrontend                             | Фреймворк на котором основан сайт                                                  |
| NodeJS          | AnalyticsBackend, EventSaver, ModNotification | Основа бэкендов                                                                    |
| ClickHouse      | AnalyticsBackend, EventSaver                  | Колоночная СУБД для сохранения событий                                             |
| Clickhouse Bulk | EventSaver                                    | Прокси сервер для СУБД для групповой вставки                                       |
| Redis           | EventSaver                                    | Кеш для сохранения пар `id_боя:token`, хранится 1 час, на случай перезахода в игру |
| MongoDB         | AnalyticsBackend                              | Сохраняет пермалинки на состояние бэкенда                                          |

***

## Принцип работы
[Мод](https://github.com/WOT-STAT/WOTMOD) собирает игровые события и отправляет их на [сервер](https://github.com/WOT-STAT/EventSaver), которых сохраняет их в базу данных.

[Сайт](https://github.com/WOT-STAT/AnalyticsFrontend) обращается к [серверу](https://github.com/WOT-STAT/AnalyticsBackend), который выгружает события из базы данных. 
