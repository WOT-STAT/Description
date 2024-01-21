# Схема таблиц в БД

Используется база данных [clickhouse](https://clickhouse.com/docs).

Параметры для подключения:
- аккаунт: public
- пароль: *отсутствует*
- база данных: WOT
- порт: 8123
- хост: dev.db.wotstat.soprachev.com

Попробовать можно тут: https://dev.db.wotstat.soprachev.com/play?user=public#c2VsZWN0ICogZnJvbSBkZXNjcmlwdGlvbg==

## Список таблиц

| Таблица                                 | Описание                   |
| --------------------------------------- | -------------------------- |
| [Description](#description)             | Список колонок с описанием |
| [OnBattleStart](#event_onbattlestart)   | События начала боя         |
| [OnShot](#event_onshot)                 | Выстрелы                   |
| [OnBattleResult](#event_onbattleresult) | События завершения боя     |

### Description
| Колонка | Тип    | Описание         |
| ------- | ------ | ---------------- |
| table   | String | Название таблицы |
| name    | String | Название колонки |
| type    | String | Тип              |
| comment | String | Комментарий      |

### Event_OnBattleStart

| Колонка           | Тип             | Описание                                        |
| :---------------- | :-------------- | :---------------------------------------------- |
| id                | UInt128         | Id события                                      |
| dateTime          | DateTime64\(3\) | Время события на сервере                        |
| battleTime        | Int32           | Время относительно начала боя в мс              |
| arenaId           | UInt64          | Id арены                                        |
| gameplayMask      | UInt32          | Маска режимов игры \(хз зачем нужно\)           |
| loadBattlePeriod  | Enum8\(...)     | Стадия боя в момент прогрузки                   |
| inQueueWaitTime   | UInt32          | Время в очереди перед началом боя в мс          |
| loadTime          | UInt32          | Время в загрузки карты в мс                     |
| preBattleWaitTime | UInt32          | Время в ожидания начала боя после загрузки в мс |
| arenaTag          | String          | Название карты                                  |
| playerName        | String          | Ник игрока                                      |
| playerClan        | String          | Клан игрока                                     |
| accountDBID       | UInt64          | ID аккаунта                                     |
| battleMode        | Enum8\(...\)    | Тип боя                                         |
| battleGameplay    | Enum8\(...\)    | Режим игры                                      |
| serverName        | String          | Название сервера                                |
| region            | String          | Регион                                          |
| gameVersion       | String          | Версия игры                                     |
| modVersion        | String          | Версия мода                                     |
| team              | UInt8           | Команда                                         |
| tankTag           | String          | Название танка                                  |
| tankType          | Enum8\(...\)    | Тип танка                                       |
| tankLevel         | UInt8           | Уровень танка                                   |
| gunTag            | String          | Тег орудия                                      |
| spawnPoint\_x     | Float32         | Координата X точки спавна                       |
| spawnPoint\_y     | Float32         | Координата Y точки спавна                       |
| spawnPoint\_z     | Float32         | Координата Z точки спавна                       |


### Event_OnShot

| Колонка                      | Тип                         | Описание                                                                 |
| :--------------------------- | :-------------------------- | :----------------------------------------------------------------------- |
| id                           | UInt128                     | Id события                                                               |
| onBattleStartId              | UInt128                     | Id события начала боя                                                    |
| localtime                    | DateTime64\(3\)             | Время события у клиента                                                  |
| dateTime                     | DateTime64\(3\)             | Время события на сервере                                                 |
| shotId                       | UInt32                      | Id выстрела из танков                                                    |
| arenaTag                     | String                      | Название карты                                                           |
| playerName                   | String                      | Ник игрока                                                               |
| playerClan                   | String                      | Клан игрока                                                              |
| accountDBID                  | UInt64                      | ID аккаунта                                                              |
| battleMode                   | Enum8\(...\)                | Тип боя                                                                  |
| battleGameplay               | Enum8\(...\)                | Режим игры                                                               |
| serverName                   | String                      | Название сервера                                                         |
| region                       | String                      | Регион                                                                   |
| gameVersion                  | String                      | Версия игры                                                              |
| modVersion                   | String                      | Версия мода                                                              |
| team                         | UInt8                       | Команда                                                                  |
| tankTag                      | String                      | Название танка                                                           |
| tankType                     | Enum8\(...\)                | Тип танка                                                                |
| tankLevel                    | UInt8                       | Уровень танка                                                            |
| gunTag                       | String                      | Тег орудия                                                               |
| battleTime                   | Int32                       | Время относительно начала боя в мс                                       |
| health                       | UInt32                      | ХП игрока в момент выстрела                                              |
| shellTag                     | Enum8\(...\)                | Тип снаряда                                                              |
| shellName                    | String                      | Название снаряда                                                         |
| shellDamage                  | Decimal\(9, 1\)             | Средний снаряда                                                          |
| damageRandomization          | Decimal\(9, 2\)             | Разброс урона (+- 25%)                                                   |
| shellPiercingPower           | Decimal\(9, 1\)             | Среднее пробитие снаряда                                                 |
| shellCaliber                 | Float32                     | Калибр снаряда                                                           |
| shellSpeed                   | UInt16                      | Скорость снаряда                                                         |
| shellMaxDistance             | UInt16                      | Максимальная дистанция полета снаряда                                    |
| gunDispersion                | Float32                     | Актуальный разброс орудия на момент выстрела \(например с учётом стана\) |
| battleDispersion             | Float32                     | Разброс орудия на момент старта боя                                      |
| serverShotDispersion         | Float32                     | Сведение орудия на момент выстрела по серверному прицелу                 |
| clientShotDispersion         | Float32                     | Сведение орудия на момент выстрела по клиентскому прицелу                |
| ballisticResultServer\_r     | Float32                     | Расстояние от серверного маркера до точки попадания                      |
| ballisticResultServer\_theta | Float32                     | Угол между осью X и вектором от серверного маркера до точки попадания    |
| ballisticResultClient\_r     | Float32                     | Расстояние от клиентского маркера до точки попадания                     |
| ballisticResultClient\_theta | Float32                     | Угол между осью X и вектором от клиентского маркера до точки попадания   |
| gravity                      | Float32                     | Гравитация                                                               |
| serverAim                    | Bool                        | Использовался ли серверный прицел                                        |
| autoAim                      | Bool                        | Использовался ли автоприцел                                              |
| ping                         | Float32                     | Пинг                                                                     |
| fps                          | UInt16                      | ФПС                                                                      |
| hitVehicleDescr              | Nullable\(UInt32\)          | Дескриптор танка, в который попал снаряд                                 |
| hitChassisDescr              | Nullable\(UInt32\)          | Дескриптор шасси танка, в который попал снаряд                           |
| hitTurretDescr               | Nullable\(UInt32\)          | Дескриптор башни танка, в который попал снаряд                           |
| hitGunDescr                  | Nullable\(UInt32\)          | Дескриптор орудия танка, в который попал снаряд                          |
| hitTurretYaw                 | Nullable\(Float32\)         | Угол поворота башни танка, в который попал снаряд                        |
| hitTurretPitch               | Nullable\(Float32\)         | Угол наклона башни танка, в который попал снаряд                         |
| vehicleDescr                 | UInt32                      | Дескриптор своего танка                                                  |
| chassisDescr                 | UInt32                      | Дескриптор шасси своего танка                                            |
| turretDescr                  | UInt32                      | Дескриптор башни своего танка                                            |
| gunDescr                     | UInt32                      | Дескриптор орудия своего танка                                           |
| turretYaw                    | Float32                     | Угол поворота башни своего танка                                         |
| turretPitch                  | Float32                     | Угол наклона башни своего танка                                          |
| shellDescr                   | UInt32                      | Дескриптор снаряда                                                       |
| hitSegment                   | Nullable\(String\)          | Сегмент, в который попал снаряд                                          |
| vehicleSpeed                 | Float32                     | Скорость своего танка                                                    |
| vehicleRotationSpeed         | Float32                     | Скорость поворота своего танка                                           |
| turretSpeed                  | Float32                     | Скорость поворота башни своего танка                                     |
| gunPoint\_x                  | Float32                     | Координата X точки выстрела                                              |
| gunPoint\_y                  | Float32                     | Координата Y точки выстрела                                              |
| gunPoint\_z                  | Float32                     | Координата Z точки выстрела                                              |
| clientMarkerPoint\_x         | Float32                     | Координата X клиентского маркера                                         |
| clientMarkerPoint\_y         | Float32                     | Координата Y клиентского маркера                                         |
| clientMarkerPoint\_z         | Float32                     | Координата Z клиентского маркера                                         |
| serverMarkerPoint\_x         | Float32                     | Координата X серверного маркера                                          |
| serverMarkerPoint\_y         | Float32                     | Координата Y серверного маркера                                          |
| serverMarkerPoint\_z         | Float32                     | Координата Z серверного маркера                                          |
| tracerStart\_x               | Float32                     | Координата X начала трассера                                             |
| tracerStart\_y               | Float32                     | Координата Y начала трассера                                             |
| tracerStart\_z               | Float32                     | Координата Z начала трассера                                             |
| tracerEnd\_x                 | Float32                     | Координата X конца трассера                                              |
| tracerEnd\_y                 | Float32                     | Координата Y конца трассера                                              |
| tracerEnd\_z                 | Float32                     | Координата Z конца трассера                                              |
| tracerVel\_x                 | Float32                     | Скорость трассера по X                                                   |
| tracerVel\_y                 | Float32                     | Скорость трассера по Y                                                   |
| tracerVel\_z                 | Float32                     | Скорость трассера по Z                                                   |
| hitReason                    | Enum8\(...\)                | Причина завершения полета снаряда                                        |
| hitPoint\_x                  | Nullable\(Float32\)         | Координата X точки попадания                                             |
| hitPoint\_y                  | Nullable\(Float32\)         | Координата Y точки попадания                                             |
| hitPoint\_z                  | Nullable\(Float32\)         | Координата Z точки попадания                                             |
| **results.\***               |                             | Результаты выстрела по танкам.                                           |
| results.order                | Array\(UInt16\)             | Индекс попадания (например попал по второму рикошетом от первого)        |
| results.tankTag              | Array\(String\)             | Название танка                                                           |
| results.shotDamage           | Array\(UInt16\)             | Урон снарядом                                                            |
| results.fireDamage           | Array\(UInt16\)             | Урон огнём                                                               |
| results.shotHealth           | Array\(Nullable\(UInt16\)\) | ХП после выстрела                                                        |
| results.fireHealth           | Array\(Nullable\(UInt16\)\) | ХП после завершения пожара                                               |
| results.ammoBayDestroyed     | Array\(Bool\)               | Взорвался ли БК                                                          |
| results.flags                | Array\(UInt16\)             | Флаги попадания (вроде бы можно вытащить криты)                          |

### Event_OnBattleResult

| name                                        | type                                      | comment                                          |
| :------------------------------------------ | :---------------------------------------- | :----------------------------------------------- |
| id                                          | UInt128                                   | Id события                                       |
| onBattleStartId                             | UInt128                                   | Id события начала боя                            |
| dateTime                                    | DateTime64\(3\)                           | Время события на сервере                         |
| result                                      | Enum8\('lose' = 0, 'win' = 1, 'tie' = 2\) | Результат боя                                    |
| credits                                     | Int32                                     | Заработанные кредиты                             |
| originalCredits                             | Int32                                     | Заработанные кредиты без према и прочего         |
| duration                                    | UInt16                                    | Длительность боя в секундах                      |
| teamHealth                                  | Array\(UInt16\)                           | Здоровье команд                                  |
| winnerTeam                                  | UInt16                                    | Победившая команда                               |
| playerTeam                                  | UInt8                                     | Команда игрока                                   |
| arenaTag                                    | String                                    | Название карты                                   |
| playerName                                  | String                                    | Ник игрока                                       |
| playerClan                                  | String                                    | Клан игрока                                      |
| accountDBID                                 | UInt64                                    | ID аккаунта                                      |
| battleMode                                  | Enum8\(...\)                              | Тип боя                                          |
| battleGameplay                              | Enum8\(...\)                              | Режим игры                                       |
| serverName                                  | String                                    | Название сервера                                 |
| region                                      | String                                    | Регион                                           |
| gameVersion                                 | String                                    | Версия игры                                      |
| modVersion                                  | String                                    | Версия мода                                      |
| team                                        | UInt8                                     | Команда                                          |
| tankTag                                     | String                                    | Название танка                                   |
| tankType                                    | Enum8\(...\)                              | Тип танка                                        |
| tankLevel                                   | UInt8                                     | Уровень танка                                    |
| gunTag                                      | String                                    | Тег орудия                                       |
|                                             |                                           |                                                  |
| **playersResults.\***                       |                                           | Результаты игроков                               |
| playersResults.team                         | Array\(UInt8\)                            | Номер команды                                    |
| playersResults.name                         | Array\(String\)                           | Ник игрока                                       |
| playersResults.bdid                         | Array\(UInt64\)                           | ID игрока                                        |
| playersResults.tankTag                      | Array\(String\)                           | Название танка                                   |
| playersResults.tankType                     | Array\(Enum8\(...\)\)                     | Типа танка                                       |
| playersResults.tankLevel                    | Array\(UInt8\)                            | Уровень танка                                    |
| playersResults.lifeTime                     | Array\(UInt16\)                           | Время жизни                                      |
| playersResults.kills                        | Array\(UInt16\)                           | Всего фрагов                                     |
| playersResults.mileage                      | Array\(UInt16\)                           | Пройденная дистанция                             |
| playersResults.damageAssistedTrack          | Array\(UInt16\)                           | Урон по сбитой гусинице                          |
| playersResults.damageAssistedRadio          | Array\(UInt16\)                           | Урон по разведданным                             |
| playersResults.directHitsReceived           | Array\(UInt16\)                           | Получено поподаний                               |
| playersResults.piercingsReceived            | Array\(UInt16\)                           | Получено пробитий                                |
| playersResults.explosionHitsReceived        | Array\(UInt16\)                           | Получено попаданий сплешем                       |
| playersResults.damageReceived               | Array\(UInt16\)                           | Полученный дамаг                                 |
| playersResults.damageReceivedFromInvisibles | Array\(UInt16\)                           | Полученный дамаг из инвиза                       |
| playersResults.directEnemyHits              | Array\(UInt16\)                           | Попаданий по противникам                         |
| playersResults.piercingEnemyHits            | Array\(UInt16\)                           | Пробито противников                              |
| playersResults.explosionHits                | Array\(UInt16\)                           | Попаданий сплешем                                |
| playersResults.damaged                      | Array\(UInt16\)                           | Число получивших урон                            |
| playersResults.shots                        | Array\(UInt16\)                           | Всего выстрелов                                  |
| playersResults.damageDealt                  | Array\(UInt16\)                           | Урона нанесено                                   |
| playersResults.stunned                      | Array\(UInt16\)                           |                                                  |
| playersResults.stunDuration                 | Array\(Float32\)                          | Время стана (либо получил либо нанёс, не помню)  |
| playersResults.spotted                      | Array\(UInt16\)                           | Скольких подсветил                               |
| playersResults.damageBlockedByArmor         | Array\(UInt16\)                           | Натанковано                                      |
| playersResults.xp                           | Array\(UInt16\)                           | Получено опыта                                   |
| playersResults.damageAssistedStun           | Array\(UInt16\)                           |                                                  |
| playersResults.killerIndex                  | Array\(UInt8\)                            | Индеск убийцы или -1 если жив                    |
| playersResults.maxHealth                    | Array\(UInt16\)                           | ХП игрока в начале боя                           |
| playersResults.health                       | Array\(UInt16\)                           | Оставшиеся ХП игрока на момент завершения боя    |
| playersResults.isAlive                      | Array\(Bool\)                             | Жив ли игрок на момент завершения боя            |
|                                             |                                           |                                                  |
| **personal.\***                             |                                           | Личные показатели. Тоже самое что playersResults |
| personal.team                               | Array\(UInt8\)                            | Номер команды                                    |
| personal.tankTag                            | Array\(String\)                           | Название танка                                   |
| personal.tankType                           | Array\(Enum8\(...\)\)                     | Типа танка                                       |
| personal.tankLevel                          | Array\(UInt8\)                            | Уровень танка                                    |
| personal.lifeTime                           | Array\(UInt16\)                           | Время жизни                                      |
| personal.kills                              | Array\(UInt16\)                           | Всего фрагов                                     |
| personal.mileage                            | Array\(UInt16\)                           | Пройденная дистанция                             |
| personal.damageAssistedTrack                | Array\(UInt16\)                           | Урон по сбитой гусинице                          |
| personal.damageAssistedRadio                | Array\(UInt16\)                           | Урон по разведданным                             |
| personal.directHitsReceived                 | Array\(UInt16\)                           | Получено поподаний                               |
| personal.piercingsReceived                  | Array\(UInt16\)                           | Получено пробитий                                |
| personal.explosionHitsReceived              | Array\(UInt16\)                           | Получено попаданий сплешем                       |
| personal.damageReceived                     | Array\(UInt16\)                           | Полученный дамаг                                 |
| personal.damageReceivedFromInvisibles       | Array\(UInt16\)                           | Полученный дамаг из инвиза                       |
| personal.directEnemyHits                    | Array\(UInt16\)                           | Попаданий по противникам                         |
| personal.piercingEnemyHits                  | Array\(UInt16\)                           | Пробито противников                              |
| personal.explosionHits                      | Array\(UInt16\)                           | Попаданий сплешем                                |
| personal.damaged                            | Array\(UInt16\)                           | Число получивших урон                            |
| personal.shots                              | Array\(UInt16\)                           | Всего выстрелов                                  |
| personal.damageDealt                        | Array\(UInt16\)                           | Урона нанесено                                   |
| personal.stunned                            | Array\(UInt16\)                           |                                                  |
| personal.stunDuration                       | Array\(Float32\)                          | Время стана (либо получил либо нанёс, не помню)  |
| personal.spotted                            | Array\(UInt16\)                           | Скольких подсветил                               |
| personal.damageBlockedByArmor               | Array\(UInt16\)                           | Натанковано                                      |
| personal.xp                                 | Array\(UInt16\)                           | Получено опыта                                   |
| personal.damageAssistedStun                 | Array\(UInt16\)                           |                                                  |
| personal.killerIndex                        | Array\(UInt8\)                            | Индеск убийцы или -1 если жив                    |
| personal.maxHealth                          | Array\(UInt16\)                           | ХП начале боя                                    |
| personal.health                             | Array\(UInt16\)                           | Оставшиеся ХП игрока на момент завершения боя    |
| personal.isAlive                            | Array\(Bool\)                             | Жив  момент завершения боя                       |

