# Bitrovo: Bitrix Panel

<p align="center">
  <img src="https://bitrovo.ru/images/cover_panel.jpg" alt="Интерфейс Bitrovo: Bitrix Panel" width="920">
</p>

Панель управления серверным окружением сайтов на «1С-Битрикс» и порталов
«Битрикс24». Веб-интерфейс для сайтов, служб, PHP, файлов, задач и обращений
в техническую поддержку.

Текущая версия: **1.0.193**.

> Этот репозиторий содержит **только документацию, лицензию и историю
> изменений**. Исходный код панели не публикуется. Устанавливается подписанный
> релиз с официального CDN.

Официальная страница: [Bitrovo:Bitrix Panel](https://bitrovo.ru/solutions/bitrixpanel)

## Установка

На сервере от root:

```bash
curl -fsSL https://cdn.bitrovo.ru/bitrovo-bitrix-panel/install.sh \
  -o /tmp/bitrovo-panel-install.sh \
  && sudo bash /tmp/bitrovo-panel-install.sh --full --yes
```

Проверка без изменений на сервере:

```bash
curl -fsSL https://cdn.bitrovo.ru/bitrovo-bitrix-panel/install.sh \
  -o /tmp/bitrovo-panel-install.sh \
  && sudo bash /tmp/bitrovo-panel-install.sh --panel-only --dry-run
```

Установщик проверяет подпись release manifest и SHA-256 архива. Неподписанный
или повреждённый архив не запускается. Существующие сайты, nginx-конфиги и базы
данных установщик не заменяет: панель ставится рядом. Если порт 443 уже занят
(типично для BitrixVM), панель открывается на отдельном HTTPS-порту
(по умолчанию `9443`).

Подробности: [установка](docs/INSTALL.md).

## Документация

- [Установка](docs/INSTALL.md) — требования, первый вход, обновление, удаление
- [Диагностика](docs/TROUBLESHOOTING.md) — пароль администратора, HTTPS, BitrixVM
- [Поддерживаемые ОС](docs/SUPPORTED-OS.md)
- [Проверенные ОС](docs/VERIFIED-OS.md) — что реально прогонялось
- [Конфиденциальность](docs/PRIVACY.md)
- [Политика безопасности](SECURITY.md)
- [Лицензия](LICENSE)

## Что умеет панель

- сайты «1С-Битрикс» и связанные службы;
- PHP, веб-сервер, базы, Redis, Push / RTC;
- файловый менеджер;
- фоновые задачи с журналом;
- обращения в техническую поддержку из интерфейса панели.

Панель не требует чистого сервера и не является обёрткой над меню BitrixVM:
ставится рядом с уже работающими сайтами.

## Обновление и удаление

```bash
bitrovo-panel-update
bitrovo-panel-uninstall
```

Сайты и базы при удалении панели не затрагиваются. Полные команды и откат —
в [INSTALL.md](docs/INSTALL.md).

## Поддержка

Вопросы по установке и работе панели принимаются через раздел «Поддержка»
внутри панели и через сайт [bitrovo.ru](https://bitrovo.ru).

Ошибки в документации можно оформить issue в этом репозитории. Не публикуйте
пароли, токены, ключи и содержимое `/etc/bitrovo-panel`.

Сообщения об уязвимостях — только по [SECURITY.md](SECURITY.md), не в открытых
issues.

## Лицензия

Bitrovo: Bitrix Panel — проприетарное программное обеспечение.
Условия использования — в [LICENSE](LICENSE).
