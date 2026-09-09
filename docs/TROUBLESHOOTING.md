# Диагностика

## Забыли пароль администратора

Через браузер восстановить пароль нельзя. Нужен root на сервере:

```bash
bitrovo-panel-reset-admin
```

Это ярлык на скрипт. То же самое полным путём:

```bash
/opt/bitrovo-panel/current/scripts/reset-admin.sh
```

Команда спросит подтверждение и новый пароль (скрытый ввод). Она:

- остановит службу панели;
- сделает резервную копию `panel.json`;
- **сбросит пароль существующего пользователя** (логин тот же; второго админа не создаёт);
- сбросит сессии и блокировки входа по IP;
- снова запустит службу.

Сайты, базы и настройки не трогает. Если пользователей несколько:

```bash
bitrovo-panel-reset-admin --username=admin
```

Если пользователей ещё нет — создаст первого администратора.

Проверка ярлыка:

```bash
ls -l /usr/sbin/bitrovo-panel-reset-admin
```

## Панель не открывается

```bash
systemctl status bitrovo-panel.service
systemctl status bitrovo-panel-root-helper.service
curl -fsS http://127.0.0.1:3847/health
nginx -t
```

API не должен быть доступен по `PUBLIC_IP:3847`. Внешний адрес панели:
`https://PUBLIC_IP/panel/` (или `https://PUBLIC_IP:9443/panel/`, если при
установке порт 443 был занят другим сайтом — см. `publicUrl` в
`/etc/bitrovo-panel/install-manifest.json`).

## Ошибка HTTPS

```bash
systemctl status bitrovo-panel-cert-renew.timer
/opt/bitrovo-panel/shared/certbot-venv/bin/certbot certificates
journalctl -u bitrovo-panel-cert-renew.service
```

Проверьте, что публичный IP не изменился, порт 80 доступен и запросы
`/.well-known/acme-challenge/` не перехватываются внешним proxy.

Если браузер предупреждает о самоподписанном сертификате, значит при
установке не удалось выпустить IP-сертификат Let's Encrypt (например, нет
Python 3.10+). Установите Python 3.10+ и выполните обновление панели —
установщик повторит выпуск сертификата.

## BitrixVM / чужой сайт на 443

На BitrixVM и похожих серверах, где 443 уже занят чужим SSL-сайтом,
установщик **сам** переносит панель на порт **9443**, открывает его в
firewall и в конце выводит правильный URL (`https://IP:9443/panel/`).

Ручной `PANEL_HTTPS_PORT` нужен только если хотите другой порт или если
стоит старая версия установщика без автоопределения:

```bash
curl -fsSL https://cdn.bitrovo.ru/bitrovo-bitrix-panel/install.sh \
  -o /tmp/bitrovo-panel-install.sh \
  && sudo PANEL_HTTPS_PORT=9443 bash /tmp/bitrovo-panel-install.sh --full --yes
```

Проверка:

```bash
curl -fsS https://PUBLIC_IP:9443/panel/ -o /dev/null -w '%{http_code}\n'
cat /etc/bitrovo-panel/install-manifest.json
```

## Ошибка root-helper

```bash
systemctl status bitrovo-panel-root-helper.service
ls -l /run/bitrovo-panel/helper.sock
journalctl -u bitrovo-panel-root-helper.service
```

Не публикуйте `ROOT_HELPER_TOKEN` из `/etc/bitrovo-panel/config.env`.

## Не синхронизируется поддержка

Обращения не теряются и остаются локально. Проверьте `SUPPORT_HUB_URL`, DNS,
исходящий HTTPS и журналы API:

```bash
journalctl -u bitrovo-panel.service
```

Outbox повторяет отправку автоматически с увеличивающейся задержкой.

## Неудачное обновление

Автоматический rollback выполняется установщиком. Для ручного переключения:

```bash
bitrovo-panel-update --rollback
```

Backup данных находится в `/opt/bitrovo-panel/shared/backups`.
