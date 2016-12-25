---
layout: single
title: Download Hardella
permalink: /download/
lang: en
---
{% capture file_prefix %}Hardella-{{ site.data.download.current_version }}{% endcapture%}
{% capture download_root %}https://github.com/Hardella/ide61131/releases/download/v{{ site.data.download.current_version }}/{{ file_prefix }}{% endcapture %}

Current version: Hardella {{ site.data.download.current_version }} (see [changelog](/docs/changelog/))

| Operating system | File | Size | Checksum |
|----------------------|------|--------|-------------------|
{% for file in site.data.download.files %}| {{ file.title }} | [{{ file_prefix }}-{{ file.suffix }}]({{ download_root }}-{{ file.suffix }}) | {{ file.size }} | [{{ file_prefix }}-{{ file.suffix }}.sha256]({{ download_root }}-{{ file.suffix }}.sha256)<br>[{{ file_prefix }}-{{ file.suffix }}.asc]({{ download_root }}-{{ file.suffix }}.asc) |
{% endfor %}

Java 8 is required to run Hardella IDE. You can get Java from [oracle.com](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (pick Java SE Development Kit).

Hardella requires no installation: you need just [unzip it](/docs/installation) an that is it.


Please check Hardella [tutorials](/docs/pru/examples/four-blinkning-leds/)


GPG signatures are generated via 2048R/410C47B1 Vladimir Sitnikov's key (fingerprint = 1A60 90D3 223D CA95 BFD2  5C0E 38F4 7D3E 410C 47B1). You can grab public key from [MIT key server](http://pgp.mit.edu/) if you want to check `.asc` signatures.
