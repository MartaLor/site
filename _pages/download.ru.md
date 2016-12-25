---
layout: single
title: Загрузить Hardella
permalink: /download/
lang: ru
---
{% capture file_prefix %}Hardella-{{ site.data.download.current_version }}{% endcapture%}
{% capture download_root %}https://github.com/Hardella/ide61131/releases/download/v{{ site.data.download.current_version }}/{{ file_prefix }}{% endcapture %}

Текущая версия: Hardella {{ site.data.download.current_version }} (см [список изменений](/docs/changelog))

| Операционная система | Файл | Размер | Контрольная сумма |
|----------------------|------|--------|-------------------|
{% for file in site.data.download.files %}| {{ file.title }} | [{{ file_prefix }}-{{ file.suffix }}]({{ download_root }}-{{ file.suffix }}) | {{ file.size }} | [{{ file_prefix }}-{{ file.suffix }}.sha256]({{ download_root }}-{{ file.suffix }}.sha256)<br>[{{ file_prefix }}-{{ file.suffix }}.asc]({{ download_root }}-{{ file.suffix }}.asc) |
{% endfor %}

Для работы Hardella IDE нужна Java 8. Скачать java можно с [сайта oracle.com](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (нужная ссылка -- Java SE Development Kit).

Инструкция по установке проста: [достаточно распаковать Hardella](/docs/installation) и всё.


После загрузки можете ознакомиться с [примерами PRU программ](/docs/pru/examples/four-blinkning-leds/)

GPG подписи созданы ключём 2048R/410C47B1 Vladimir Sitnikov (fingerprint = 1A60 90D3 223D CA95 BFD2  5C0E 38F4 7D3E 410C 47B1). Если хотите проверить `.asc` подпись, можете загрузить ключ с [сервера MIT](http://pgp.mit.edu/).
