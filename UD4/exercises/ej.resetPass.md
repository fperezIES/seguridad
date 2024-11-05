---
title: Restauración de contraseñas
---


<!--
Esta práctica se debe acompañar de dos máquinas virutales:

Una debian 10 con un password en root seguro
Una con Windows 10 con un usuario que sea administrador seguro
 * En mi caso el usuario es fperez

En cada página se pondrá un fondo de pantalla distinto en los usuarios objetivo para asegurar que el reseteo de password ha sido realizado satisfactoriamente. 

PWD máquinas: gatoperro1
-->
# Restaurar contraseña de root en Debian


![Password reset](../img/resetPass/rst_passwd.png){:style="width: 40%;" class="center"}


Hoy vamos a ver los problemas que presenta que una persona tenga acceso físico a una máquina.

* Empezaremos comprobando los problemas de **no asegurar el GRUB** en una máquina **Linux**.

* Después veremos que es muy sencillo **cambiar la contraseña a un usuario local de Windows**.


> Puedes descargar las máquinas para probar de los siguientes enlaces:
> 
> - [Debian](https://drive.google.com/file/d/1Ug51cI5QU_JRXc5AYDtzopPzgialnQ-m/view?usp=drive_link)
> - [Windows 10](https://drive.google.com/file/d/154_79tj0vYpHCziFwr08BgwjeX9F2V1R/view?usp=sharing)
> 


Si tenemos acceso físico a la máquina resulta relativamente sencillo restaurar la contraseña del usuario root siguiendo este tutorial:

> 1) Debes conseguir acceder a la máquina que te proporcione el profesor en clase, con el usuario **root** y hacer una captura del escritorio para demostrar que has conseguido hacer login. 

Vamos a modificar el script de arranque para conseguir privilegios de administrador sin tener ninguna contraseña.

Para ello arranca o reinicia tu Debian. Cuando aparezca el menú de arranque **GRUB**. Teniendo seleccionada la primera opción, pulsa la tecla `e` en el teclado antes de que el sistema arranque. Con este paso podremos editar los parámetros del kernel antes de que el sistema arranque.

![Menú GRUB](../img/resetPass/deb10_grub.jpg){:style="width: 80%;" class="center"}

Esto hará que se muestre una pantalla como la que se muestra a continuación. Localiza la línea que empieza con  `linux` que precede la sección `/boot/vmlinuz-*` que también especifica el UUID.

![Menú GRUB](../img/resetPass/deb10_grup_kernel_params.jpg){:style="width: 80%;" class="center"}

Lleva el cursor al final de esta línea, justo antes de `ro quiet` y añade el parámetro `init=/bin/bash`

A continuación presiona `ctrl + x` para hacer que el sistema arranque en modo de usuario único, con el el sistema de ficheros montado con permisos de sólo lectura (`ro`)

Para cambiar el password, necesitas cambiar los permisos de acceso al sistema de fichero de solo lectura (`ro`) a lectura y escritura (`rw`). Para ello usa el siguiente comando:

```sh
mount -o remount,rw /
```

Ahora ya puedes cambiar el password simplemente usando el comando `passwd`. 

```sh
passwd
```

Proporciona el nuevo password, vuélvelo a introducir para confirmarlo y ya está.

Reinicia la máquina para comprobar que ahora puedes hacer login como **root** con tu nuevo password.

> 2) Hemos podido hacer todo este procedimiento porque no se ha protegido el GRUB. ¿Cómo podría añadir una contraseña al menú GRUB para evitar que alguien use este procedimiento?

# Resetear contraseña de usuario en Windows 10

Usaremos la herramienta Hirens Boot CD [(HBCD: https://www.hirensbootcd.org/)](https://www.hirensbootcd.org/). Es un DVD auto-arrancable con herramientas gratuítas y legales que permiten resolver gran cantidad de problemas en equipos con Windows instalado.

En nuestro caso vamos a usar una herramienta de *Lazesoft* para resetear la contraseña de una máquina con Windows 10 que te proporcionará el profesor en clase. 

> 3) Debes conseguir acceder a la máquina con el usuario que te indique el profesor y hacer una captura del escritorio para demostrar que has conseguido hacer login con el usuario indicado. 

El proceso es muy sencillo:

* Arranca la máquina con HBCD
	* Como trabajamos con máquinas virtuales, es suficiente con que cargues el fichero ISO en la unidad de DVD de la máquina virtual. 
* Ejecuta la Herramienta `Lacesoft Password Recovery`
* Sigue el asistente para reiniciar la contraseña
* Reinicia la máquina y haz login con la cuenta a la que has cambiado la contraseña 

![Lacesoft Password Recovery](../img/resetPass/HBCD_lacesoft.png){:style="width: 80%;" class="center"}

Tienes un tutorial completo en el siguiente enlace: [https://www.lazesoft.com/how-to-reset-administrator-password.html](https://www.lazesoft.com/how-to-reset-administrator-password.html)

![Lacesoft Password Recovery](../img/resetPass/HBCD_user_reset.png){:style="width: 80%;" class="center"}

> 4 ) Para realizar este procedimiento en una máquina física necesitarías grabar la imagen en un DVD o preparar un USB de arranque. ¿Cómo se puede preparar un USB de arranque a partir de un fichero ISO?.


# Bibliografía

- [https://www.hirensbootcd.org/](https://www.hirensbootcd.org/)

- [https://www.lazesoft.com/how-to-reset-administrator-password.html](https://www.lazesoft.com/how-to-reset-administrator-password.html)

- [https://www.tecmint.com/reset-forgotten-root-password-in-debian/](https://www.tecmint.com/reset-forgotten-root-password-in-debian/)

- [https://askubuntu.com/questions/24006/how-do-i-reset-a-lost-administrative-password](https://askubuntu.com/questions/24006/how-do-i-reset-a-lost-administrative-password)

- [https://geekland.eu/proteger-el-grub-con-contrasena/](https://geekland.eu/proteger-el-grub-con-contrasena/)

- [http://www.gnu.org/software/grub/](https://geekland.eu/proteger-el-grub-con-contrasena/)

### Creación de boot USB a partir de ISOs: 

- [https://www.top-password.com/iso2disc.html](https://www.top-password.com/iso2disc.html)

- [https://rufus.ie/](https://rufus.ie/)

