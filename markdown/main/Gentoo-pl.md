<!-- source: https://wiki.gentoo.org/wiki/Gentoo-pl | group: Gentoo Wiki (Main) | wiki-title: Gentoo-pl -->
---
title: Gentoo-pl
url: https://wiki.gentoo.org/wiki/Gentoo-pl
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-05-29"
fingerprint: "478d18dc9f07415e"
license: CC BY-SA 4.0
---

# Gentoo-pl

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Witaj na stronie kanału [#gentoo-pl w sieci Libera.Chat](irc://irc.libera.chat/gentoo-pl)!**

Prosimy o zapoznanie się z [regulaminem kanału](https://wiki.gentoo.org/wiki/Gentoo-pl/regulamin) lub przynajmniej [podstawowymi zasadami](https://wiki.gentoo.org#Podstawowe_zasady) wymienionymi poniżej.

Kanał powstał w celu wymiany wiedzy i doświadczeń pomiędzy polskimi użytkownikami Gentoo. Należy do polskiej społeczności Gentoo i przez tę społeczność jest zarządzany. Jest kanałem oficjalnym, zarejestrowanym przez zespół Gentoo Developer Relations 22 marca 2003 roku. Władze kanału to najbardziej doświadczeni użytkownicy tej dystrybucji oraz osoby aktywnie ją rozwijające.

## Podstawowe zasady

1. Tak jak na wszystkich kanałach Libera.Chat używamy kodowania UTF-8.
2. Używamy polskich znaków, vide [Łona - ĄĘ](https://www.youtube.com/watch?v=T2iISWltdzc).
3. Do wklejania treści dłuższych niż 2 linijki polecamy [wklej.org](http://wklej.org).
4. Linki dłuższe niż 64 znaki skracamy w [tiny.pl](http://tiny.pl) lub podobnym serwisie.
5. Linki, których nie wypada otwierać w pracy, należy otagować `nsfw` lub `nws`.
6. Nieznajomość [pełnego regulaminu](https://wiki.gentoo.org/wiki/Gentoo-pl/regulamin) nie zwalnia z jego przestrzegania.

## Naucz się pisać po polsku z #gentoo-pl

1. Nie piszemy spacji przed znakami interpunkcyjnymi.
2. Piszemy „na razie” i „na pewno”.
3. Na temat „tę” i „tą”:
4. Front ochrony dopełniacza:
5. [Czcionka, font, krój czy typeface? O zagmatwanym nazewnictwie](http://wizualny.wordpress.com/2013/05/20/czcionka-font-kroj-typeface-nazewnictwo/)

## FAQ

### Jak ustawić polskie znaki w X-ach?

Utwórz plik /etc/X11/xorg.conf.d/10-keyboard.conf, jeśli go jeszcze nie masz, i ustaw `XkbLayout` na `pl`, jak niżej:

**`/etc/X11/xorg.conf.d/10-keyboard.conf`**

```
Section "InputClass"
        Identifier "keyboard-all"
        Driver "evdev"
        Option "XkbLayout" "pl"
        Option "XkbVariant" "basic"
        MatchIsKeyboard "on"
EndSection
```
Następnie zrestartuj X-y.

### Popsuły mi się sterowniki od X-ów, co robić?

Na przyszłość zainstaluj sobie [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) i używaj `eread` do przeglądania logów po aktualizacji, a na razie masz:

`root #``emerge @x11-module-rebuild`
### Jak ustawić alternatywną mapę do X-ów?

Zrobimy to na przykładzie mapy antoszki.

1. [http://antoszka.pl/plantoni](http://antoszka.pl/plantoni) zapisz sobie jako plantoni w katalogu /share/X11/xkb/symbols/.
2. Utwórz plik /etc/X11/xorg.conf.d/10-keyboard.conf, jeśli go jeszcze nie masz, i ustaw `XkbLayout` na `plantoni`, jak niżej:

**`/etc/X11/xorg.conf.d/10-keyboard.conf`**

```
Section "InputClass"
        Identifier "keyboard-all"
        Driver "evdev"
        Option "XkbLayout" "plantoni"
        Option "XkbVariant" "basic"
        MatchIsKeyboard "on"
EndSection
```
## Przydatne linki

1. Alternatywna mapa pl do X-ów: [http://antoszka.pl/plantoni](http://antoszka.pl/plantoni)
2. Pokrzaczyły się polskie literki? [http://krzaki.blizinski.pl/](http://krzaki.blizinski.pl/)
3. [http://without-systemd.org/](http://without-systemd.org/)
