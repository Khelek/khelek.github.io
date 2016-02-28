---
layout: post
title:  "Включение на клавиатуре Microsoft Natural Ergonomic Keyboard 4000 ползунка масштабирования, на Ubuntu 12.10"
categories: other
published: true
---

После приобретения шикарной клавиатуры [Microsoft Natural Ergonomic Keyboard 4000](http://www.microsoft.com/hardware/en-us/p/natural-ergonomic-keyboard-4000/B2M-00012#details) я столкнулся с тем, что под Ubuntu на ней не работал скроллер, расположенный в самом её центре.
 <!--more-->
{% fullwidth "https://www.microsoft.com/hardware/_base_v1/products/natural-ergonomic-keyboard-4000/mk_nek4000v2_large.jpg" %}
Немного прошерстив интернет, я выяснил, что некоторые клавиши этой клавы генерируют события с кодами выше некоего значения (фиг знает какого, пусть будет 255).

А потом я натолкнулся на этот [пост](http://rebelliard.com/blog/enabling-scrolling-using-the-microsoft-natural-ergonomic-keyboard-4000s-zoom-slider-on-ubuntu-1210/), где чувак заменял нестандартные события этой клавы на свои. Так вот:

### Простой способ

1. Откройте файл /lib/udev/rules.d/95-keymap.rules:

    ```
    $ sudo nano /lib/udev/rules.d/95-keymap.rules
    ```
2. Находим строку, начинающуюся с ENV{ID_VENDOR}=="Microsoft". У меня это выглядит так:

    ```
    ENV{ID_VENDOR}=="Microsoft", ENV{ID_MODEL_ID}=="00db", RUN+="keymap $name 0xc022d zoomin 0xc022e zoomout"
    ```
3. Замените zoomin значение с pageup и zoomout значение с pagedown (или на чтото своё, мне для емакса было удобнее переназначить на leftctrl и rightctrl ):

    ```
    ENV{ID_VENDOR}=="Microsoft", ENV{ID_MODEL_ID}=="00db", RUN+="keymap $name 0xc022d pageup 0xc022e pagedown"
    ```
4. Изменения будут применены, если переподключить клавиатуру или перезагрузить комп.

P.S. В статье чувака также есть длинный путь для тех, у кого не получилось простым.
P.P.S. Работает также в Ubuntu 12.04 и 13.10.
