<div align="center">

# SHUI Uploader Plugin

**Плагин Cura для загрузки G-code на WiFi-принтеры SHUI**


</div>

Плагин устройства вывода для Ultimaker Cura, который загружает нарезанный G-code на 3D-принтеры SHUI с WiFi по HTTP. Включает Qt GUI (PyQt6 с фолбэком на PyQt5) с управлением несколькими принтерами, панелью управления принтером, TCP-консолью, уведомлениями в Telegram и интеграцией с Яндекс Алисой / Яндекс Диском. Также работает автономно и как скрипт постобработки PrusaSlicer.

## ■ Возможности

- ❖ **Интеграция с Cura** — регистрируется как устройство вывода в меню сохранения/экспорта
- ❖ **WiFi-загрузка** — HTTP POST G-code на эндпоинт `/upload` принтера по локальной сети
- ❖ **Несколько принтеров** — управление и выбор из нескольких настроенных принтеров (обычный HTTP или режим ESP32)
- ❖ **Управление принтером** — джоггинг осей, запуск фрагментов G-code, просмотр температуры хотенда и стола
- ❖ **Вкладка консоли** — живой терминал через TCP-сокет к принтеру (порт 8080)
- ❖ **Уведомления в Telegram** — оповещения о статусе печати через Telegram-бота (опционально, при наличии настроек)
- ❖ **Интеграция с Алисой** — голосовые сценарии Яндекс Алисы с загрузкой на Яндекс Диск (опционально, при наличии настроек)
- ❖ **Поддержка PrusaSlicer** — работает как автономный скрипт постобработки

## ■ Стек

<div align="center">

| Компонент | Технология |
|-----------|------------|
| Язык | Python 3 |
| GUI | PyQt6 / PyQt5 |
| Cura SDK | 7.5 -- 8.2 |
| Загрузка | HTTP (QtNetwork) |
| Консоль | TCP socket |

</div>

## ■ Как это работает

```
1. Установите директорию плагина в папку plugins Cura; он регистрируется как устройство вывода в меню сохранения/экспорта.
2. Настройте принтеры через config.json (обычный HTTP или режим ESP32) и выберите цель в Qt GUI.
3. Нарежьте модель в Cura и запустите загрузку — G-code отправляется HTTP POST на эндпоинт /upload принтера по локальной сети.
4. Управляйте принтером (джоггинг осей, запуск фрагментов G-code, мониторинг температуры хотенда и стола) или откройте живую TCP-консоль (порт 8080) из панели плагина.
5. При желании получайте уведомления в Telegram о статусе печати или загружайте на Яндекс Диск для запуска голосовых сценариев Яндекс Алисы.
```

## ■ Использование

Скопируйте директорию плагина в папку `plugins` Cura. Для автономного запуска из корня плагина:

```bash
pip install -r requirements.txt
START_MODE=STANDALONE python prusha.py <gcode_file>
```

Без `START_MODE=STANDALONE` `prusha.py` запускается в режиме постобработки PrusaSlicer.

## ■ Структура репозитория

```
.
├── __init__.py          # Cura plugin entry (registers output_device)
├── ShuiPlugin.py        # OutputDevicePlugin / OutputDevice implementation
├── prusha.py            # standalone / PrusaSlicer entry point
├── plugin.json          # Cura plugin manifest
├── config.json          # printers, proxy, Telegram, Yandex, snippets
├── requirements.txt     # PyQt5
└── shui/
    ├── MainUI.py        # main Qt window, App state, start modes
    ├── PyQt_API.py      # PyQt6-then-PyQt5 import shim
    ├── langs.json       # ru / ua / en strings
    └── utils/
        ├── Core.py              # StartMode, UI base classes, preview
        ├── WifiSender.py        # HTTP G-code upload
        ├── WifiUart.py          # TCP console connection thread
        ├── FileSaver.py         # sender base classes
        ├── CuraGCodeParser.py   # Cura G-code source
        ├── PrusaGcodeParser.py  # Prusa G-code source
        ├── YandexSender.py      # Yandex Disk / Alisa upload
        ├── FileTab.py / ConsoleTab.py / PrinterControlTab.py
        ├── TelegramTab.py / AlisaTab.py / SetupDialog.py
        └── controls/            # GCodeActionsControl (jog/snippets)
```

## ■ Лицензия

MIT © [pluttan](https://github.com/pluttan)
