# Сервер
Есть 2 вида сервера 
- dev версия
- release версия 

Доступ к микросвервисам осуществляется путём проксирования запросов на разные порты

| Сервис                                                           | dev порт | release порт |
| ---------------------------------------------------------------- | -------- | ------------ |
| [EventSaver](https://github.com/WOT-STAT/EventSaver)             | 4101     | 5101         |
| [AnalyticsBackend](https://github.com/WOT-STAT/AnalyticsBackend) | 4102     | 5102         |
| [ModNotification](https://github.com/WOT-STAT/ModNotification)   | 4103     | 5103         |

