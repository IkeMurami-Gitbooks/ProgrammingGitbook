# Chrome Cast

Это протокол-технология. В частности по ней можно управлять телевизорами и другими устройствами (на базе Android?).

Chromecast устройство может запускать специальные приложения — Receiver'ы.

Документация: [https://developers.google.com/cast](https://developers.google.com/cast)

Консоль разработчика: [https://cast.google.com/u/2/publish/#/overview](https://cast.google.com/u/2/publish/#/overview)

User-case: пользователь на телефоне щелкает воспроизвести фильм, телевизор его воспроизводит.

Есть одна небольшая проблема, как это тестировать, не имея телевизор? Сам не пробовал, но нашел следующие решения (TODO: проверить):

* [https://casttool.appspot.com/cactool/](https://casttool.appspot.com/cactool/)
* [https://www.npmjs.com/package/chromecast-device-emulator](https://www.npmjs.com/package/chromecast-device-emulator)
