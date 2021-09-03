---
layout: single
title: Tutorial: memoria USB multiboot todoterreno (sin programas dedicados)
excerpt: "En esta entrada os enseñaré a preparar una socorrida memoria USB que podrá contener y ejecutar tantas imágenes distros GNU/Linux como pueda almacenar. Tendremos la opción tanto instalarla como de usarla en modo live. Y por si esto os pareciera poco, será compatible tanto con ordenadores de 32 y 64 bits y que tengan BIOS o UEFI."
date: 2017-06-03
classes: wide
header:
  teaser: /assets/images/jinkros/2017-06-03-tutorial-memoria-usb-multiboot-todoterreno-sin-programas-dedicados/wallpaper.jpg
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - Jinkros
  - Tutoriales
  - Hardware
  - Distros y Entornos
tags:
  - Almacenamiento
  - Antergos
  - Apricity
  - Arch
  - ArchLinux
  - Arch Linux
  - CloneZilla
  - Como Hacer un USB Multiboot
  - Debian
  - Devuan
  - Distro
  - Entorno
  - Distros y Entornos
  - Linux
  - GNU/Linux
  - GNU
  - Fedora
  - GParted
  - Hardware
  - Kali
  - Kali Linux
  - KaliLinux
  - Kubuntu
  - Live
  - Manjaro
  - Memria USB
  - Mint
  - MultiBoot
  - MultiBoot USB
  - Parrot
  - PenDrive
  - Sabayon
  - SystemRescueCD
  - Tutoriales
  - Ubuntu
  - USB
  - WifiSlax
  - Xubuntu
---

![](/assets/images/jinkros/2017-06-03-tutorial-memoria-usb-multiboot-todoterreno-sin-programas-dedicados/grub.png)

**IMPORTANTE: Este post se actualiza para ir añadiendo entradas de grub según sea pedido en los comentarios y grupo de telegram. Por favor tenlo en cuenta para consultas futuras.**

En esta entrada os enseñaré a preparar una socorrida memoria USB que podrá contener y ejecutar tantas imágenes distros GNU/Linux como pueda almacenar. Tendremos la opción tanto instalarla como de usarla en modo live. Y por si esto os pareciera poco, será compatible tanto con ordenadores de 32 y 64 bits y que tengan BIOS o UEFI. Pasos a seguir:

**NOTA QUE DEBÉIS TENER EN MENTE SIEMPRE CUANDO LEÁIS TUTORIALES DE GNU/LINUX: los comandos precedidos por # deben ser ejecutados como usuario root y los precedidos por $ deben ser ejecutados por un usuario no root.**

**Paso 0:** Conseguir una memoria USB y un sistema GNU/Linux que tenga grub2 instalado. Si no teneis grub2 en vuestro sistema podéis, o instalarlo y desinstalarlo cuando acabéis el proceso o hacerlo desde otra distro en modo live que venga con grub2 de serie por ejemplo [Antergos](http://antergos.com/try-it) o [Manjaro](http://manjaro.org/get-manjaro/).

**Paso 1:** Una vez insertado el pendrive objetivo debemos anotar muy bien que nombre de dispositivo le ha asignado nuestro sistema, para ello en la consola ejecutamos el comando *lsblk* y nos aparecerá algo parecido a esto:

```
user@Antergos  ~  lsblk   
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 111,8G  0 disk 
├─sda1   8:1    0   100M  0 part 
├─sda2   8:2    0  55,9G  0 part 
└─sda3   8:3    0  55,9G  0 part 
sdb      8:16   0 931,5G  0 disk 
├─sdb1   8:17   0    12G  0 part [SWAP]
├─sdb2   8:18   0  47,7M  0 part /boot/efi
├─sdb3   8:19   0  46,6G  0 part /
└─sdb4   8:20   0 872,9G  0 part /media/Datitos
sdc      8:32   1  28,9G  0 disk 
└─sdc1   8:33   1  28,9G  0 part
```

Como podéis ver hay tres dispositivos detectados (sda/b/c), mi pendrive es el dispositivo ***sdc*** (podréis reconocer el vuestro por el tamaño y porque en la columna RM que significa *removable* tendrá un 1) así que a partir de ahora cuando veáis que pone ***sdc*** no olvidéis cambiarlo por el identificador de vuestro dispositivo antes de ejecutar cualquier comando o puede que echéis a perder vuestro sistema. Ahora es cuando os digo que no olvidéis hacer copia de los datos importantes antes de seguir. A continuación abrimos GParted, en la parte superior derecha seleccionamos nuestro pendrive (/dev/***sdc***) y en la barra superior abrimos la opción **Dispositivo** y elegimos **Crear tabla de particiones**, aparecerá una ventana como en la imagen en la que debemos seleccionar **msdos** y aplicar. Una vez creada la tabla de particiones tenemos que formatear dicha memoria en fat32. Y sí, fat32 para evitar problemas posteriormente, el tamaño máximo de los archivos que podréis meter será de 4GB pero es raro encontrar isos live que pesen tanto.

![](/assets/images/jinkros/2017-06-03-tutorial-memoria-usb-multiboot-todoterreno-sin-programas-dedicados/gparted.png)

**Paso 2:** Ya tenemos el pendrive formateado y con la tabla de particiones correcta, ahora nos toca pasar a la terminal para ejecutar comandos (¡¡YUHUUUU!!). Sacamos nuestro pendrive, esperamos unos segundos y lo volvemos a enchufar, en la terfminal ejecutamos de nuevo *lsblk* y miramos el punto de montaje de nuestro pendrive (aparece en la columna MOUNTPOINT). Si nuestro sistema no lo ha automontado lo montamos nosotros, ejecutando *lsblk* para ver si se sigue llamando ***sdc*** o ha cambiado de nombre y después el siguiente comando, si ha pasado a llamarse sdd pues a partir de ahora ponemos sdd y listo:

```
# mount /dev/sdc1 /mnt/MULTIBOOT 
```

Ahora que tenemos montada nuestra memoria USB le debemos meter grub2 con los siguientes comandos, primero para sistemas UEFI. Si fue automontada recuerda cambiar ***/mnt/MULTIBOOT*** por el punto de montaje que tu sistema le haya asignado:

```
# grub-install --target=x86_64-efi --efi-directory=/mnt/MULTIBOOT --boot-directory=/mnt/MULTIBOOT/boot --removable --recheck
```

Ahora para sistemas BIOS. De nuevo recuerda cambiar ***/mnt/MULTIBOOT*** por el punto de montaje que tu sistema le haya asignado y ***sdc*** por el identificador de tu pendrive:

```
# grub-install --target=i386-pc --boot-directory=/mnt/MULTIBOOT/boot --recheck /dev/sdc
```

Con esto ya será tu memoria USB arrancable desde BIOS y UEFI. El resto del tutorial será meter las imágenes iso y editar el archivo *grub.cfg*.

**Paso 3:** Abrimos la carpeta raíz de nuestro pendrive (en nuestro caso ***/mnt/MULTIBOOT***) y entramos en la carpeta boot que habrá, dentro de esta creamos una carpeta llamada *iso* que será la encargada de albergar todas las distros que queramos tener. Pegamos dentro de dicha carpeta las isos pertinentes y seguimos al siguiente paso (ten paciencia, algunas pueden tardar mucho en pegarse y más si no usas una memoria USB 3.0 o superior).

![](/assets/images/jinkros/2017-06-03-tutorial-memoria-usb-multiboot-todoterreno-sin-programas-dedicados/rutasmultiboot.png)

**Paso 4:** Ahora toca preparar el menú de grub2. De nuevo abrimos la consola y ejecutamos *lsblk -f* para averiguar el UUID de la partición fat32 de nuestro dispositivo.

```
user@Antergos  ~  lsblk -f
NAME   FSTYPE LABEL     UUID                                 MOUNTPOINT
sda                                                          
├─sda1 vfat   ESP       EF27-8CD6                            
├─sda2 ext4   ArchLinux 1c589cc4-c921-4358-9a33-d18b13a238fc 
└─sda3 ntfs   Windows10 231AEE697FFBBBDB                     
sdb                                                          
├─sdb1 swap             283afb8d-13de-4ed3-9d9d-415e2ed8ed8f [SWAP]
├─sdb2 vfat   ESP2      69E3-56C5                            /boot/efi
├─sdb3 ext4   Antergos  23e5b6dc-1438-45a6-8584-b6935c24ec52 /
└─sdb4 ntfs   Datitos   607A29E42CA2A820                     /media/Datitos
sdc                                                          
└─sdc1 vfat             AD7A-F378                            /mnt/MULTIBOOT
```

Como podéis ver el UUID de mi partición es ***AD7A-F378***, desde ahora debéis sustituir ese UUID cuando aparezca por el de vuestra partición.

Vamos a la carpeta ***/mnt/MULTIBOOT****/boot/grub* y buscamos el archivo grub.cfg, si no está lo creamos nosotros y pegamos en su interior lo siguiente (sustituye mi UUID por el tuyo en la primera línea):

```
set imgdevpath='/dev/disk/by-uuid/AD7A-F378'
menu_color_highlight=light-green/black
menu_color_normal=white/black
if [ "${grub_platform}" == "efi" ]; then
    insmod efi_gop
    insmod efi_uga
else
    insmod vbe
    insmod vga
fi
insmod gfxterm
terminal_output gfxterm
loadfont /boot/grub/fonts/unicode.pf2
```

Si queremos ponerle un fondo al grub debemos pegar la imagen que queramos de fondo en la carpeta *grub* de nuestro dispositivo (la carpeta *grub* está dentro de la carpeta *boot*) y añadir estas líneas debajo de las que acabamos de pegar:

```
#insmod png
#insmod jpeg
background_image -m stretch /boot/grub/imagen.png
```

Si tu fondo de grub es una imagen en formato png debes eliminar la # de la primera línea y si está en formato jpg/jpeg debes eliminar la # de la segunda línea.

En la tercera línea va la ruta al archivo de imagen pero sólo desde la raíz de tu dispositivo hasta el propio archivo, es decir, en lugar de */mnt/MULTIBOOT/boot/grub/imagen.png* debemos poner */boot/grub/imagen.png*. No olvidéis sustituir la extensión .png por la de vuestro archivo.

A continuación tenemos que crear las entradas del menú para las distros que hayamos metido en la carpeta *iso*. Para que quede “bonito” añadimos una línea vacía debajo de lo que acabamos de poner en *grub.cfg* y de los siguientes cuadros pegamos los que necesitemos. Recordad cambiar lo que hay entre las comillas simples de la primera y segunda línea de cada apartado, en la primera debe ir el nombre que queráis que aparezca en el menú de grub2 y en la segunda debe ir la ruta desde la raíz del pendrive hasta vuestro archivo .iso:

- **[Arch Linux](https://www.archlinux.org/):**

```
menuentry 'archlinux-2017.06.01-x86_64.iso' {
    set isofile='/boot/iso/archlinux-2017.06.01-x86_64.iso'
    loopback loop $isofile
    linux (loop)/arch/boot/x86_64/vmlinuz img_dev=$imgdevpath img_loop=$isofile earlymodules=loop
    initrd (loop)/arch/boot/x86_64/archiso.img
}
```

- **[Antergos](https://www.antergos.com/):**

```
menuentry 'antergos-17.5-x86_64.iso' {
    set isofile='/boot/iso/antergos-17.5-x86_64.iso'
    loopback loop $isofile
    linux (loop)/arch/boot/vmlinuz img_dev=$imgdevpath img_loop=$isofile earlymodules=loop
    initrd (loop)/arch/boot/archiso.img
}
```

- **[Apricity OS](https://lignux.com/apricity-os-echa-el-cierre/):**

```
menuentry 'apricity_os-11.2016-birch-gnome-x86_64.iso' {
    set isofile='/boot/iso/apricity_os-11.2016-birch-gnome-x86_64.iso'
    loopback loop $isofile
    linux (loop)/arch/boot/x86_64/vmlinuz img_dev=$imgdevpath img_loop=$isofile earlymodules=loop
    initrd (loop)/arch/boot/x86_64/archiso.img
}
```

NOTA: no importa que sea la iso con GNOME o Cinnamon, simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

NOTA: el link de Apricity OS lleva a una entrada de lignux.com en la que podéis encontrar el link de descarga de ambas versiones de Apricity OS ya que su web oficial está cerrada.

- **[Manjaro](https://manjaro.github.io/):**

```
menuentry 'manjaro-kde-17.0.1-stable-x86_64.iso' --class dvd {
    set isofile='/boot/iso/manjaro-kde-17.0.1-stable-x86_64.iso'
    set dri="nonfree"
    search --no-floppy -f --set=root $isofile
    loopback loop $isofile
    linux  (loop)/boot/vmlinuz-x86_64  img_dev=$imgdevpath img_loop=$isofile driver=$dri
    initrd  (loop)/boot/intel_ucode.img (loop)/boot/initramfs-x86_64.img
}
```

NOTA: no importa el entorno de escritorio que tenga la iso (KDE, GNOME, XFCE…), simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

- **[Debian](https://www.debian.org/):**

```
menuentry 'debian-live-8.8.0-amd64-lxde-desktop.iso' {
    set isofile='/boot/iso/debian-live-8.8.0-amd64-lxde-desktop.iso'
    loopback loop $isofile
    linux (loop)/live/vmlinuz boot=live config findiso=$isofile desktop=lxde priority=low
    initrd (loop)/live/initrd.img
}
```

NOTA: no importa el entorno de escritorio que tenga la iso (KDE, GNOME, XFCE…), simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

- **[Devuan](https://devuan.org/):**

```
menuentry 'devuan_jessie_1.0.0_amd64_uefi_desktop-live.iso' {
    set isofile='/boot/iso/devuan_jessie_1.0.0_amd64_uefi_desktop-live.iso'
    loopback loop $isofile
    linux (loop)/live/vmlinuz boot=live config findiso=$isofile
    initrd (loop)/live/initrd.img
}
```

- **[Kali Linux](https://www.kali.org/):**

```
menuentry 'kali-linux-2017.1-amd64.iso' {
    set isofile='/boot/iso/kali-linux-2017.1-amd64.iso'
    loopback loop $isofile
    linux (loop)/live/vmlinuz boot=live findiso=$isofile noconfig=sudo hostname=kali
    initrd (loop)/live/initrd.img
}
```

NOTA: el usuario root del live de Kali Linux es ‘root’ y su contraseña ‘toor’.

NOTA: no importa el entorno de escritorio que tenga la iso (KDE, GNOME, XFCE…), simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

- **[Parrot](https://www.parrotsec.org/):**

```
menuentry 'Parrot-full-3.6_amd64.iso' {
    set isofile='/boot/iso/Parrot-full-3.6_amd64.iso'
    loopback loop $isofile
    linux (loop)/live/vmlinuz boot=live findiso=$isofile noconfig=sudo hostname=parrot
    initrd (loop)/live/initrd.img
}
```

NOTA: el usuario root del live de Parrot es ‘root’ y su contraseña ‘toor’.

- **[Ubuntu](https://www.ubuntu.com/):**

```
menuentry 'ubuntu-17.04-desktop-amd64.iso' {
    set isofile='/boot/iso/ubuntu-17.04-desktop-amd64.iso'
    loopback loop $isofile
    linux (loop)/casper/vmlinuz.efi boot=casper iso-scan/filename=$isofile
    initrd (loop)/casper/initrd.lz
}
```

- **[Ubuntu Studio](https://ubuntustudio.org/):**

```
menuentry 'ubuntustudio-17.04-dvd-amd64.iso' {
    set isofile='/boot/iso/ubuntustudio-17.04-dvd-amd64.iso'
    loopback loop $isofile
    linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=$isofile
    initrd (loop)/casper/initrd.lz
}
```

- **[Kubuntu](https://www.kubuntu.org/)/[Xubuntu](https://www.xubuntu.org/)/[Ubuntu MATE](https://ubuntu-mate.org/)/[Ubuntu GNOME](https://ubuntugnome.org/)/Lubuntu:**

```
menuentry 'kubuntu-17.04-desktop-amd64.iso' {
    set isofile='/boot/iso/kubuntu-17.04-desktop-amd64.iso'
    loopback loop $isofile
    linux (loop)/casper/vmlinuz.efi boot=casper iso-scan/filename=$isofile
    initrd (loop)/casper/initrd.lz
}
```

NOTA: no importa el entorno de escritorio que tenga la iso (KDE, GNOME, XFCE…), simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

- **[Linux Mint](https://www.linuxmint.com/):**

```
menuentry 'linuxmint-18.1-cinnamon-64bit.iso' {
    set isofile='/boot/iso/linuxmint-18.1-cinnamon-64bit.iso'
    loopback loop $isofile
    linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=$isofile noeject noprompt 
    initrd (loop)/casper/initrd.lz
}
```

NOTA: no importa el entorno de escritorio que tenga la iso (KDE, GNOME, XFCE…), simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

- **[Zorin OS](https://zorinos.com/) (core):**

```
menuentry 'Zorin-OS-12.1-Core-64.iso' {
    set isofile='/boot/iso/Zorin-OS-12.1-Core-64.iso'
    loopback loop $isofile
    linux   (loop)/casper/vmlinuz boot=casper iso-scan/filename=$isofile quiet splash
    initrd  (loop)/casper/initrd.lz
}
```

- **[Void Linux](http://www.voidlinux.eu/):**

```
menuentry 'void-live-x86_64-20170220-xfce.iso' {
    set isofile='/boot/iso/void-live-x86_64-20170220-xfce.iso'
    loopback loop $isofile
    linux (loop)/boot/vmlinuz iso-scan/filename=$isofile root=live:CDLABEL=VOID_LIVE
    initrd (loop)/boot/initrd
}
```

NOTA: no importa el entorno de escritorio que tenga la iso (KDE, GNOME, XFCE…), simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

- **[Fedora Workstation](https://getfedora.org/):**

```
menuentry 'Fedora-Workstation-Live-x86_64-25-1.3.iso' {
    set isofile='/boot/iso/Fedora-Workstation-Live-x86_64-25-1.3.iso'
    loopback loop $isofile
    linux (loop)/isolinux/vmlinuz root=live:CDLABEL=Fedora-WS-Live-25-1-3 iso-scan/filename=$isofile rd.live.image quiet
    initrd (loop)/isolinux/initrd.img
}
```

- **[Sabayon Linux](https://www.sabayon.org/):**

```
menuentry 'Sabayon_Linux_16.11_amd64_Xfce.iso' {
    insmod loopback
    set isofile=/boot/iso/Sabayon_Linux_16.11_amd64_Xfce.iso
    loopback loop ${isofile}
    linux (loop)/boot/sabayon root=/dev/ram0 init=/linuxrc isoboot=$isofile cdroot looptype=squashfs loop=/livecd.squashfs overlayfs
    initrd (loop)/boot/sabayon.igz
}
```

NOTA: no importa el entorno de escritorio que tenga la iso (KDE, GNOME, XFCE…), simplemente debes cambiar lo que he mencionado antes de la primera y segunda línea.

- **[Clonezilla](http://clonezilla.org/):**

```
menuentry 'clonezilla-live-2.5.0-25-amd64.iso' {
    set isofile='/boot/iso/clonezilla-live-2.5.0-25-amd64.iso'
    loopback loop $isofile
    linux (loop)/live/vmlinuz boot=live live-config noswap findiso=$isofile union=overlay username=user config acpi_osi= noapic nolapic —
    initrd (loop)/live/initrd.img
}
```

- **[GParted](http://gparted.org/livecd.php):**

```
menuentry 'gparted-live-0.28.1-1-amd64.iso' {
    set isofile='/boot/iso/gparted-live-0.28.1-1-amd64.iso'
    loopback loop $isofile
    linux (loop)/live/vmlinuz boot=live union=overlay username=user config components quiet noswap noeject toram=filesystem.squashfs nosplash findiso=$isofile iso-scan/filename=$
    initrd (loop)/live/initrd.img
}
```

- **[SystemRescueCD](http://www.system-rescue-cd.org/):**

```
menuentry 'systemrescuecd-x86-5.0.1.iso' {
    set isofile='/boot/iso/systemrescuecd-x86-5.0.1.iso'
    loopback loop $isofile
    linux (loop)/isolinux/rescue64 setkmap=es isoloop=$isofile
    initrd (loop)/isolinux/initram.igz
}
```

- **[Wifislax](http://www.wifislax.com/):**

```
menuentry "Wifislax64 Live UEFI ($sl_lang)" {
    set isofile='/boot/iso/wifislax64-1.0-final.iso'
    loopback loop $isofile
    linux (loop)/boot/vmlinuz kbd=$sl_kbd tz=$sl_tz locale=$sl_locale xkb=$sl_xkb rw
    initrd (loop)/boot/initrd.xz
}
```

```
menuentry "Wifislax64 Live UEFI En RAM ($sl_lang)" {
    set isofile='/boot/iso/wifislax64-1.0-final.iso'
    loopback loop $isofile
    linux (loop)/boot/vmlinuz kbd=$sl_kbd tz=$sl_tz locale=$sl_locale xkb=$sl_xkb rw toram
    initrd (loop)/boot/initrd.xz
}
```

- **Reiniciar:**

```
menuentry 'Reiniciar' {
    reboot
}
```

 

- **Apagar:**

```
menuentry 'Apagar' {
    halt
}
```

 

Y fin, llegados a este punto si habéis seguido los pasos como se han indicado debéis tener un fantástico pendrive listo para enchufar y arrancar con una multitud de distros.

Si echáis de menos alguna distro decídmelo en los comentarios y la intentaré añadir tan pronto como sea posible.

Extra: quizás os hayáis dado cuenta pero además de usarla como memoria USB multiboot, podréis usarla también para almacenar vuestros archivos como con cualquier otro pendrive.

Fondo de grub2 usado en la primera imagen:

[![](/assets/images/jinkros/2017-06-03-tutorial-memoria-usb-multiboot-todoterreno-sin-programas-dedicados/wallpaper.jpg)

Fuentes y agradecimientos: la increíble [wiki de Arch Linux](https://wiki.archlinux.org/index.php/Multiboot_USB_drive) (en la que colaboro y os animo a colaborar a vosotros en lo que podáis, ya que una parte de este post es gracias a ella) y la infinita sabiduría del compañero Felfa ya que este post es tan suyo como mío (de no haber sido por su ayuda aún estaría dandome de tortas con grub2 para que funcionase correctamente en la memoria USB).

Cuéntanos en los comentarios tu experiencia y si fue mala te ayudaremos a solucionar el problema que tuvieras ^^.

Eso es todo. Propicios días.
