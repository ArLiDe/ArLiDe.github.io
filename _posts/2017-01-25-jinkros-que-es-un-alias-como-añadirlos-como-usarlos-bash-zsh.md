---
layout: single
title: Qué es un alias, cómo añadirlos y usarlos (BASH y ZSH)
excerpt: "Un ***alias*** será un *apodo* que usaremos para invocar un comando. Un alias puede estar formado por una o varias palabras, números o caracteres (normalmente es una combinación corta, lógica y/o fácil de recordar). Según el uso que le demos a los alias pueden llegar a ahorrarnos mucho tiempo a la hora de manejar nuestra terminal en GNU/Linux o simplemente los podemos usar para personalizar aún más nuestro sistema."
date: 2017-01-10
classes: wide
header:
  teaser: /assets/images/jinkros/2017-01-25-que-es-un-alias-como-añadirlos-como-usarlos-bash-zsh/aliasdef.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - @Jinkros
  - Tutoriales
  - Programas
tags:
  - .bashrc
  - .zsh
  - Alias
  - Bash
  - Zsh
  - Zshell
  - Command
  - Como Añadir Alias
  - Como Usar Alias
  - Consola
  - Terminal
  - GNU
  - GNU/Linux
  - Linux
  - RC
  - Shell
---

Un ***alias*** será un *apodo* que usaremos para invocar un comando. Un alias puede estar formado por una o varias palabras, números o caracteres (normalmente es una combinación corta, lógica y/o fácil de recordar).

![](/assets/images/jinkros/2017-01-25-que-es-un-alias-como-añadirlos-como-usarlos-bash-zsh/aliasdef.png)

Según el uso que le demos a los alias pueden llegar a ahorrarnos mucho tiempo a la hora de manejar nuestra terminal en GNU/Linux o simplemente los podemos usar para personalizar aún más nuestro sistema. Para poner esto en perspectiva haz este ejercicio mental, imagina que cada día que inicias el PC quieres actualizar tu sistema Arch Linux, para ello debes ejecutar este comando:

```
sudo pacman -Syyu --noconfirm
```

O en mi caso, este, que actualiza la lista de mirrors, la organiza y después actualiza el sistema y un framework para ZSH llamado [ZIM](https://github.com/Eriner/zim/blob/master/README.md):

```
sudo reflector --country 'France' -p https --sort rate --threads 1 --save /etc/pacman.d/mirrorlist && yaourt -Syyua --needed --noconfirm && zmanage update
```

¿Te gustaría escribir todo esto cada vez que necesites sus funciones? A mí no, por eso tengo asignado el alias 12 a ese comando, es decir, cuando en la terminal introduzco 12 y pulso enter, la shell (ZSH) lo interpreta como si realmente ese 12 fuera toooooodo el comando citado arriba, ¿útil no? Para hacer esto sólo necesitas dos datos, uno es el alias que quieres crear y el otro la shell que utilizas.

El alias que quieres es cosa tuya y la shell que usas si no estás segur@ de cual es puedes averiguarlo ejecutando en una terminal el comando:

```
echo $SHELL
```

![](/assets/images/jinkros/2017-01-25-que-es-un-alias-como-añadirlos-como-usarlos-bash-zsh/terminator_neofetch.png)

Cómo podéis ver uso ZSH, por lo tanto debo buscar *.zshrc* en mi home (es un archivo oculto) y editarlo, si usas BASH será *.bashrc*. Para editarlo vamos a la terminal y ejecutamos este comando:

```
nano ~/.zshrc
```

Lógicamente debes cambiar *.zshrc* por *.bashrc* si ese fuera tu archivo.

Cuando lo tengamos abierto en la terminal vamos al final y en una línea vacía debemos escribir lo siguiente:

```
alias alias_corto_que_vamos_a_usar="comando largo que queremos evitar"
```

Después pulsamos CTRL + O y ENTER para guardar y CTRL + X para salir, cerramos la terminal y la volvemos a abrir para que así lea de nuevo nuestro archivo configuración de shell y se tengan en cuenta nuestras modificaciones.

Para dejarlo aún más claro os muestro como quedaría la línea de mi alias:

```
alias 12="sudo reflector --country 'France' -p https --sort rate --threads 1 --save /etc/pacman.d/mirrorlist && yaourt -Syyua --needed --noconfirm && zmanage update"
```

¿Simple no? Pues además podéis añadir todos los alias que queráis, simplemente id añadiendo cada uno en una línea nueva al final de vuestro *.bashrc* o *.zshrc*. Si cambiáis de shell recordad migrar vuestros alias al archivo correspondiente de esa shell.

Os dejo una [rápida demostración del funcionamiento de los alias en un vídeo de The Linux Foundation](https://youtu.be/zN1u5xZFFR8?t=607) (si no lo hace automáticamente, ve hasta el minuto 10:07) 😉

Pues eso es todo, espero que te sea muy útil y recuerda, dar las gracias no cuesta nada y me anima a seguir escribiendo :D.