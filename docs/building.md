# Сборка QWRT

[English](building.en.md)

Сборку запускает workflow `.github/workflows/build.yml` на GitHub Actions — при
push в `main` или вручную через **Actions → Build QWRT sysupgrade → Run workflow**.
Результат — sysupgrade для односекционной разметки QWRT, manifest и SHA-256 в
артефакте Actions. Workflow не собирает factory-образ и не публикует feeds или
GitHub Releases.

Этот репозиторий содержит интеграционный слой проекта
[timofey-maykov/be7000-openwrt](https://github.com/timofey-maykov/be7000-openwrt):
`config.buildinfo`, закреплённые feeds, патчи, overlay, `awg-feed` и тему LuCI.
Само дерево OpenWrt workflow клонирует из порта
[kravasuper/openwrt](https://github.com/kravasuper/openwrt), ветка
`xiaomi_be7000`, коммит `790d036a`, как предписано исходным проектом.

Профиль `profiles/qwrt` заменяет DTS, UBI volume и обработчик sysupgrade для
установленной разметки QWRT. Он не выбирает `rootfs_1`, не меняет флаги загрузчика
и не подключает отдельный MTD `overlay`. Общие патчи проекта для драйверов,
конфигурации ядра и пакетов остаются в сборке; QWRT DTS профиль отдельно включает
исправление регулятора из патча `005`, чтобы его применение не зависело от
двухслотового DTS.

После успешного запуска скачайте `xiaomi-be7000-qwrt-squashfs-sysupgrade.bin` из
артефакта и проверьте SHA-256 по `sha256sums.txt`. Используйте образ только через
штатное обновление QWRT на устройстве с этой разметкой.
