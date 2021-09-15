# Список микросервисов WOT-STAT
Список микросервисов используемых в этом проекте.\
[Репозиторий со старой версией](https://github.com/SoprachevAK/wot-stat/tree/v1.0.0.0) 

***
## Оглавление
1. [Описание](#описание)
   1. [CI/CD](#cicd)
   2. [Стек технологий](#стек-технологий)
2. [Принцип работы](#принцип-работы)
   1. [Хранение данных](#хранение-данных)
      1. [OnBattleStart](#onbattlestart)
      2. [OnShot](#onshot)
      3. [OnBattleResult](#onbattleresult)
***

## Описание

| Сервис            | Описание                                                                               |
| ----------------- | :------------------------------------------------------------------------------------- |
| AnalyticsFrontend | Сайт отображает статистику [wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) |
| AnalyticsBackend  | Обрабатывает запросы с сайта и достаёт из БД нужные данные                             |
| WOTMOD            | Мод для WOT собирающий игровые события                                                 |
| EventSaver        | Сохраняет события из мода в БД                                                         |
| ModNotification   | Служит для поддержки автообновления мода и возможных уведомлений в моде                |
| SQL               | Различные SQL запросы для инициализации БД                                             |

## CI/CD
GitHub Actions во всех сервисах настроен так:
1. Состояние ветки **master** соответствует [**dev**.wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) поддомену.
2. Релизная версия [wotstat.soprachev.com](https://dev.wotstat.soprachev.com/) будет соответствовать релизам на гитхабе.
z
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

## Хранение данных
Так выглядят таблицы в базе данных:
Вызывается в момент перехода с PREBATTLE
### OnBattleStart:
| Название            | Тип           | Описание                                                                                                                                                                        |
| ------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                  | UUID          | ID                                                                                                                                                                              |
| dateTime            | DateTime64(3) | Дата                                                                                                                                                                            |
| arenaWGID           | UInt64        | arenaID                                                                                                                                                                         |
| arenaTag            | String        | Название карты в формате 'spaces/10_hills'                                                                                                                                      |
| battleMode          | String        | Режим игры (REGULAR/TRAINING...)                                                                                                                                                |
| battleGameplay      | Enum8(...)    | Тип геймплея (other, ctf, domination, assault, nations, ctf2, domination2, assault2, fallout, fallout2, fallout3, fallout4, ctf30x30, domination30x30, sandbox, bootcamp, epic) |
| team                | UInt8         | Номер команды                                                                                                                                                                   |
| playerWGID          | UInt32        | BDID аккаунта                                                                                                                                                                   |
| playerName          | String        | Имя аккаунта                                                                                                                                                                    |
| playerClanTag       | String        | Название клана                                                                                                                                                                  |
| tankTag             | String        | Название танка в формате 'germany:G121_Grille_15_L63'                                                                                                                           |
| tankType            | String        | Тип танка (AT/LT...)                                                                                                                                                            |
| tankLevel           | UInt8         | Уровень танка                                                                                                                                                                   |
| gunTag              | String        | Тег пушки в формате '_88mm_Pak_43_L71A'                                                                                                                                         |
| startDispersion     | Float64       | Разброс на момент старта боя, с учётом экипажа и ттх                                                                                                                            |
| loadBattlePeriod    | Enum8()       | Период боя в который произошел вход в бой (other, IDLE, WAITING, PREBATTLE, BATTLE, AFTERBATTLE)                                                                                |
| battleTimeMS        | Int32         | Время в входа в бой относительно конца обратного отсчёта                                                                                                                        |
| battleLoadTimeMS    | Int32         | Время загрузки карты                                                                                                                                                            |
| preBattleWaitTimeMS | Int32         | Время ожидания начала отсчёта                                                                                                                                                   |
| spawnPoint_x        | Float32       | Координата спавна                                                                                                                                                               |
| spawnPoint_y        | Float32       | ^                                                                                                                                                                               |
| spawnPoint_z        | Float32       | ^                                                                                                                                                                               |
| modVersion          | String        | Версия мода                                                                                                                                                                     |
| gameVersion         | String        | Версия игры                                                                                                                                                                     |
| serverName          | String        | Название сервера                                                                                                                                                                |
| region              | String        | Регион игры                                                                                                                                                                     |

### OnShot:
Вызывается на каждый выстрел

| Название             | Тип                  | Описание                                                     |
| -------------------- | -------------------- | ------------------------------------------------------------ |
| id                   | UUID                 | ID                                                           |
| onBattleStart_id     | UUID                 | ID события начала боя                                        |
| dateTime             | DateTime64(3)        | Дата                                                         |
| battleTimeMS         | Int32                | Время боя относительно конца отсчёта                         |
| shotWGID             | UInt32               | ID выстрела                                                  |
| shellTag             | String               | Тег снаряда (ARMOR_PIERCING...)                              |
| gunDispersion        | Float64              | Разброс орудия с учётом временных бафов (например оглушение) |
| battleDispersion     | Float64              | Разброс орудия на момент старта боя без бафов                |
| serverShotDispersion | Float64              | Серверное сведение                                           |
| clientShotDispersion | Float64              | Клиентское сведение                                          |
| gravity              | Float32              | Гравитация  трассера                                         |
| gunPoint_x           | Float32              | Координата пушки                                             |
| gunPoint_y           | Float32              | ^                                                            |
| gunPoint_z           | Float32              | ^                                                            |
| clientMarkerPoint_x  | Float32              | Координата клиентского маркера                               |
| clientMarkerPoint_y  | Float32              | ^                                                            |
| clientMarkerPoint_z  | Float32              | ^                                                            |
| serverMarkerPoint_x  | Float32              | Координата серверного маркера                                |
| serverMarkerPoint_y  | Float32              | ^                                                            |
| serverMarkerPoint_z  | Float32              | ^                                                            |
| tracerStart_x        | Float32              | Координата старта трассера                                   |
| tracerStart_y        | Float32              | ^                                                            |
| tracerStart_z        | Float32              | ^                                                            |
| tracerEnd_x          | Float32              | Координата конца трассера                                    |
| tracerEnd_y          | Float32              | ^                                                            |
| tracerEnd_z          | Float32              | ^                                                            |
| tracerVelocity_x     | Float32              | Вектор начальной скорости трассера                           |
| tracerVelocity_y     | Float32              | ^                                                            |
| tracerVelocity_z     | Float32              | ^                                                            |
| hitPoint_x           | Nullable(Float32)    | Координата попадания (если есть)                             |
| hitPoint_y           | Nullable(Float32)    | ^                                                            |
| hitPoint_z           | Nullable(Float32)    | ^                                                            |
| hitReason            | Nullable(Enum8(...)) | Причина попадания (tank, terrain, other)                     |
| serverAim            | UInt8                | Используется ли серверный прицел                             |
| autoAim              | UInt8                | Используется автоприцел                                      |
| ping                 | Float32              | Пинг                                                         |



### OnBattleResult:
Вызывается в конце боя если не вышел в ангар или в агаре если это результат прошлого боя


| Название         | Тип                  | Описание                                                                                            |
| ---------------- | -------------------- | --------------------------------------------------------------------------------------------------- |
| id               | UUID                 | ID                                                                                                  |
| onBattleStart_id | UUID                 | ID события начала боя                                                                               |
| dateTime         | DateTime64(3)        | Дата                                                                                                |
| reason           | Enum8(...)           | Причина завершения боя: normal, timeout(если результат боя не пришел за 60 минут после начала боя)) |
| durationMs       | Nullable(UInt32)     | Время боя                                                                                           |
| credits          | Nullable(Int16)      | Кредиты (грязные)                                                                                   |
| xp               | Nullable(UInt16)     | Опыт (грязный)                                                                                      |
| botsCount        | Nullable(UInt8)      | Количество **официальных** ботов                                                                    |
| result           | Nullable(Enum8(...)) | Результат боя (win, lose, error)                                                                    |
