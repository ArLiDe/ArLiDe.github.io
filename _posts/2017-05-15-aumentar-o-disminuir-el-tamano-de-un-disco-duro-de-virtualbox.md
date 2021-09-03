---
layout: single
title: Aumentar o disminuir el tamaño de un disco duro de VirtualBox en Linux
excerpt: "Como habéis leído en el título del post, hoy os enseñaré a aumentar o disminuir el tamaño asignado a un disco duro virtual de VirtualBox en Linux. En la siguiente imagen podéis ver como el tamaño inicial de mi disco virtual es de 1,8GB."
date: 2017-05-15
classes: wide
header:
  teaser: /assets/images/Aumentar-o-disminuir-el-tamaño-de-un-disco-duro-de-VirtualBox-en-Linux/captura-01.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
- Jinkros
- Tutoriales
  tags:
- VirtualBox
---

Como habéis leído en el título del post, hoy os enseñaré a aumentar o disminuir el tamaño asignado a un disco duro virtual de VirtualBox en Linux. En la siguiente imagen podéis ver como el tamaño inicial de mi disco virtual es de 1,8GB.

![](/assets/images/Aumentar-o-disminuir-el-tamaño-de-un-disco-duro-de-VirtualBox-en-Linux/captura-01.png)

El procedimiento es tan sencillo como ejecutar este comando en vuestra terminal favorita (adaptándolo por supuesto a vuestro caso)

```shell
VBoxManage modifyhd /RUTA/AL/ARCHIVO.vdi --resize TAMAÑONUEVOENMEGABYTES
```

Deberéis reemplazar la ruta hacia vuestro archivo de disco virtual .vdi y el tamaño que queráis que tenga el disco, recordad que para pasar de megabytes (MB) a gigabytes (GB) hay que dividir entre 1024. Tras hacer esto solo tendréis que abrir vuestra máquina virtual y reasignarle el tamaño a las particiones que os interesen desde GParted o la herramienta que incorpore el sistema que tenéis virtualizado.

![](/assets/images/Aumentar-o-disminuir-el-tamaño-de-un-disco-duro-de-VirtualBox-en-Linux/captura-02.png)

Y aquí el resultado

![](/assets/images/Aumentar-o-disminuir-el-tamaño-de-un-disco-duro-de-VirtualBox-en-Linux/captura-03.png)

No olvidéis que igual que hay que reacomodar las particiones tras aumentar el tamaño del disco virtual, si por el contrario queréis disminuir el tamaño, es recomendable usar GParted o equivalentes para disminuir el tamaño de las particiones ANTES de editar el tamaño del disco virtual y tras editarlo ver si está todo bien de nuevo con GParted o hay que agrandar las particiones por inicialmente haberlas reducido más de lo debido.

Eso es todo. Propicios días.
