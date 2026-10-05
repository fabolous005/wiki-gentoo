<!-- source: https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Japanese/es-419 | group: Gentoo Wiki (Main) | wiki-title: How to read and write in Japanese/es-419 -->
---
title: Como leer y escribir en Japonés
url: https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Japanese/es-419
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-21"
fingerprint: f156d43461abf01e
license: CC BY-SA 4.0
---

# Como leer y escribir en Japonés

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Esta guía busca explicar como leer y escribir en Japonés en un sistema no Japonés. Por favor siéntase libre de corregirlo según su conocimiento o experiencia personal.

## Requerimientos

Para soportar el lenguaje Japonés y sus caracteres, se requiere que una cierta cantidad de herramientas, librerías y recursos sean instalados en el sistema.

## Tipografías japonesas

La mayor parte de los sistemas no japoneses no tienen las tipografías japonesas instaladas. Cuando un usuario trata de introducir caracteres japoneses desde el teclado, solamente verá pequeños rectángulos en lugar de los caracteres en la pantalla.

### Japanese Menus and Environment

For those interested (perhaps for immersion based learning) in having a Japanese language based environment, in order to change menus and other materials into the Japanese language, change the user's profile into LANG=ja\_JP.UTF-8 (in the user's locale as well as .bash\_profile<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>)

## Métodos de entrada

Para leer y escribir en Japonés, lo primero que se necesita es una manera de introducir los caracteres japoneses con el teclado. Esto se logra por medio de una software usualmente llamado "método de entrada". De momento, para Japonés, hay dos métodos comunes: "anthy" y "mozc".

Con tal software escribir "ta" en el teclado introducirá el kana **た** en el procesador de texto. Una manipulación simple, que es relevante a la manera en la que los métodos de entrada funcionan, permitirá el cambio del hiragana  **た** al katakana  **タ**.

De la misma manera escribir "nihon" introducirá **にほん** y con otra simple manipulación permitirá que se convierta en la versión en kanji de la palabra, **日本**.

### IME

Además de esto, los usuarios necesitan una manera de cambiar del método de input que normalmente se usa para en lenguaje primaria, al necesario para el lenguaje Japonés. Esta funcionalidad es proporcionada por otro software llamado IME (Editor de método de input por sus siglas en inglés) tales como [app-i18n/ibus](https://packages.gentoo.org/packages/app-i18n/ibus), [app-i18n/scim](https://packages.gentoo.org/packages/app-i18n/scim) o [app-i18n/fcitx](https://packages.gentoo.org/packages/app-i18n/fcitx).

Una vez instalado, esto permite al usuario intercambiar del método de entrada de un idioma al de Japonés usando una combinación de teclas o el mouse para seleccionar el icono relevante en la bandeja de íconos.

## Instalación

### Tipografías japonesa

Como mínimo, instale el paquete [media-fonts/kochi-substitute](https://packages.gentoo.org/packages/media-fonts/kochi-substitute).

`root #``emerge --ask kochi-substitute`
Adicionalmente, están disponibles los siguientes paquetes:

## Métodos de entrada

Se recomienda utilizar "ibus" en lugar de "scim".

#### anthy

`root #``emerge --ask ibus-anthy`
#### mozc

`root #``USE=ibus emerge --ask app-i18n/mozc`
#### Configuración

`user $``ibus-setup`
En la caja de dialogo que aparece, de click en la pestaña de Input method y agregue el método "japanese-anthy". Regrese a la pestaña General y defina una combinación de teclas como un atajo del teclado para cambiar entre métodos de entrada.

The following useful keybindings could be set up for the "Japanese - Mozc" method:

Preferences → General → Keymap → Keymap style → Customize…

| Mode | Key | Command | 
|---|---|---|
| Direct input | `` Ctrl ` `` | Set input mode to Hiragana | 
| Precomposition | `` Ctrl ` `` | Deactivate IME | 

#### Common USE Flags

The following use flags are commonly employed<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>:

**cjk** - Support for Hanzi-inspired characters (containing two bytes, hence the cause of accented a's *cum* sans cjk environment)

**nls** - 'native language support' - enables other languages in interface,

**immqt-bc** - For Qt to manage other language inputs

**immqt** - conflicts with immqt-bc as of Qt3. 

**unicode** - Standard except for cursive hebrew


### Latex

Aquí hay algunos requisitos adicionales para escribir archivos de LaTeX en Japonés.

#### Soporte para CJK y xetex

Para poder escribir fragmentos de Japonés en archivos de Latex, añada soporte para los lenguajes CJK y para \[[xetex](https://en.wikipedia.org/wiki/XeTeX)\] en Texlive.

Esto se puede lograr añadiendo o modificando las siguientes lineas en /etc/portage/package.use:

**`/etc/portage/package.use/latex`**

**Habilitando soporte para cjk y xetex**

Después reinstale los paquetes:

`root #``emerge --ask --newuse app-text/texlive app-text/texlive-core`
Aquí hay una pequeña muestra funcional de LaTeX:

**`japanese.tex`**

```
\documentclass{article}
\usepackage{CJKutf8}
\usepackage{color}
 
\begin{document}
 
\begin{CJK}{UTF8}{min}
\section{Un ejemplo simple}
\textcolor{red}{これは赤いです。}
\\
私は日本語で書けます。
\\
Pero también puedo escribir en el alfabeto latino.
\end{CJK}
 
\end{document}
```
#### Configuración del editor

Para compilar y visualizar el output de la muestra sobre el editor Texmaker o Texstudio se debe de configurar apropiadamente.

Habra Texmaker, y vaya a Options -> Configure Texmaker. Debajo de la pestaña de Commands canbie lo siguiente:

- En la linea de Latex, cambie "latex" por "platex".
- En la linea Dvipdfm, cambie "divipdfm" por "dvipdfmx".

Dentro de la pestaña Fast compile, escoja "Latex + Dvipdfm + View PDF".

Finalmente vaya a la pestaña Editor, escoja UTF8 encoding y deseleccione On the fly en el linea del diccionario.
