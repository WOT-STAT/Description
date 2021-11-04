# OnBattleStart

Вызывается в момент перехода с PREBATTLE

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
| loadBattlePeriod    | Enum8(...)    | Период боя в который произошел вход в бой (other, IDLE, WAITING, PREBATTLE, BATTLE, AFTERBATTLE)                                                                                |
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
