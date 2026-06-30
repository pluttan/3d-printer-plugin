![Header](header.png)

<div align="center">

# SHUI Uploader Plugin

**Cura plugin for uploading G-code to SHUI WiFi printers**

[![License](https://img.shields.io/badge/license-MIT-2C2C2C?style=for-the-badge&labelColor=1E1E1E)](LICENSE)
[![Python](https://img.shields.io/badge/python-plugin-2C2C2C?style=for-the-badge&logo=python&labelColor=1E1E1E)]()
[![Cura](https://img.shields.io/badge/cura%20sdk-7.5--8.2-2C2C2C?style=for-the-badge&labelColor=1E1E1E)]()

</div>

An output device plugin for Ultimaker Cura that uploads sliced G-code to SHUI WiFi-connected 3D printers over HTTP. Features a Qt GUI (PyQt6 with PyQt5 fallback) with multi-printer management, a printer control panel, a TCP console, Telegram notifications, and Yandex Alisa / Yandex Disk integration. Also runs standalone and as a PrusaSlicer post-processing script.

## ■ Features

- ❖ **Cura integration** — registers as an output device in the save/export menu
- ❖ **WiFi upload** — HTTP POST of G-code to the printer's `/upload` endpoint over the local network
- ❖ **Multi-printer** — manage and select from multiple configured printers (plain HTTP or ESP32 mode)
- ❖ **Printer control** — jog axes, run G-code snippets, view hotend/bed temperature
- ❖ **Console tab** — live terminal over a TCP socket to the printer (port 8080)
- ❖ **Telegram notifications** — print status alerts via Telegram bot (optional, when configured)
- ❖ **Alisa integration** — Yandex Alisa voice scenarios with Yandex Disk upload (optional, when configured)
- ❖ **PrusaSlicer support** — runs as a standalone post-processing script

## ■ Stack

| Component | Technology |
|-----------|------------|
| Language | Python 3 |
| GUI | PyQt6 / PyQt5 |
| Cura SDK | 7.5 -- 8.2 |
| Upload | HTTP (QtNetwork) |
| Console | TCP socket |

## ■ Usage

Copy the plugin directory into Cura's `plugins` folder. To run standalone from the plugin root:

```bash
pip install -r requirements.txt
START_MODE=STANDALONE python prusha.py <gcode_file>
```

Without `START_MODE=STANDALONE`, `prusha.py` starts in PrusaSlicer post-processing mode.

## ■ Repository Structure

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

## ■ License

MIT © [pluttan](https://github.com/pluttan)
