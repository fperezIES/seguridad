---
title: Auditoría de seguridad de servidores
---



<!-- 

Referencias:


Maria DB
https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-debian-11


-->

# Introducción

Greenbone Vulnerability Manager (GVM) es una herramienta de auditoría de seguridad modular, que se utiliza para probar **sistemas remotos** en busca de vulnerabilidades que deben corregirse.

La herramienta se llamaba anteriormente **OpenVAS**.



![Logo Greenbone](img/GVM/greenbone_logo.png){width=40%}



# Enunciado

En esta ocasión vamos a intentar usar GVM para auditar una o varias máquinas remotas, pueden ser tanto linux como windows.

## Preparación de la máquina a escanear

Instala algunos servicios antes de comenzar la auditoría. Puedes usar alguna de las máquinas vituales que ya tengas de otras asignaturas, o puedes configurar una a propósito para realizar las pruebas.

Se sugiere usar alguna de las máquinas usadas previamente (Debian). En la máquina que será objetivo de la auditoría se debe instalar algunos servicios, tales como SSH, Apache, MariaDB, etc. De este modo tendremos algo que escanear con GVM.

SSH

```sh
sudo apt-get install openssh-server
```

Apache

```sh
sudo apt-get update
sudo apt-get install apache2
```

MariaDB

```sh
sudo apt update
sudo apt install mariadb-server
# Omitiremos la configuración segura para tener algo que detectar
# sudo mysql_secure_installation
```

**Revisa la bibliografía antes de empezar.**

## Instala la herramienta GVM

GVM es una herramienta que se ejecuta remotamente, así que es necesario disponer de otra máquina para poder utilizarla. En esta ocasión se propone usar una máquina virutal en la que instalaremos la distribución  Kali ([https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)) para usar la herramienta **GVM**. 

<!--
https://dannyda.com/2021/12/17/how-to-install-gvm-greenbone-vulnerability-management-openvas-on-kali-linux-2021-4/ 
-->

Para instalar y configurar GVM ejecutaremos:

```sh
sudo apt-get update
sudo apt-get install gvm
```

Para inicializar GVM:
```sh
sudo gvm-setup
```

**Este paso lleva bastante tiempo**. Al terminar la configuración **obtendremos la contraseña del usuario, debemos anotarla**. La salida por la consola será algo parecido a lo siguietne:

```sh
[+] GVM feeds updated
[*] Checking Default scanner
[*] Modifying Default Scanner
Scanner modified.

[+] Done
[*] Please note the password for the admin user
[*] User created with password 'a7dae120-f5fc-4687-8ca5-dc881e131103'.

[>] You can now run gvm-check-setup to make sure everything is correctly configured
```

Podemos comprobar que todo está correctamente instalado mediante el comando:

```sh
sudo gvm-check-setup
```

Actualizaicón de las pruebas a realizar. **No omitir este paso**.

```sh
sudo  gvm-feed-update
```


Para iniciar GVM
```sh
sudo gvm-start
```

Veremos una salida similar a:

```sh
[sudo] password for test: 
[>] Please wait for the GVM services to start. 
[>] 
[>] You might need to refresh your browser once it opens. 
[>] 
[>] Web UI (Greenbone Security Assistant): https://127.0.0.1:9392
```

Ahora podremos acceder a la interfaz web a través de  [https://127.0.0.1:9392](https://127.0.0.1:9392)  (Nota: Se debería lanzar el navegador Firefox de forma automática, si no lo hiciera podremos hacerlo de forma manual)

![GVM login page](img/GVM/GVM_1_login.png){width=50%}


Para deterner el servicio (cuando no se quiera seguir utilizando):

```sh
 sudo gvm-stop
```


Una vez instalado y configurado ya  puedes usar la herramienta para escanear las vulnerabilidades de la máquina objetivo.

## Utilización de GVM


## Crear una tarea

Debemos usar el menú `scans->tasks`, crearemos una nueva tarea utilizando el wizard y la ejecutaremos.
Podemos escanear varia máquinas.

![GVM login page](img/GVM/GVM_2_task_wizard.png){width=70%}

## Problemas al crear una tarea

Probablemente te encuentres con un problema como el siguiente:

![GVM login page](img/GVM/GVM_3_task_problem.png){width=70%}


Parece que el problema está relacionado con el cambio de Feed Owner, que debería realizarse ejecutando los comandos `gvmd` con el usuario `gvm`

Si te sucede esto se puede solucionar siguiendo los siguientes pasos: 

1 - Obtener el id de usario 

```sh
sudo runuser -u _gvm -- gvmd --get-users --verbose
```

Obtendremos algo similar a:

`admin ea55b1d6-1d15-4a0a-891c-64cfea123276`

En el siguiente comando debemos sustituir `uid` por el valor que hemos obtenido al ejecutar el comando anterior.


```sh
sudo runuser -u _gvm -- gvmd --modify-setting 78eceaec-3385-11ea-b237-28d24461215b --value <uid>
```

Con el resultado del ejemplo anteriro el comando quedaría de la siguiente forma:

`sudo runuser -u _gvm -- gvmd --modify-setting 78eceaec-3385-11ea-b237-28d24461215b --value ea55b1d6-1d15-4a0a-891c-64cfea123276`

Reiniciamos GVM:

```sh
sudo gvm-stop
sudo gvm-start
```


## Tareas a realizar

1. Instala GVM
2. Escanea un mínimo de dos máqiunas
3. Muestra los resultados de los escaneos realizados
4. Revisa los informes y comenta los problemas encontraddos


# Bibliografía

* Desarrollador:  [https://www.greenbone.net/](https://www.greenbone.net/)
* Docs: [https://greenbone.github.io/docs/latest/](https://greenbone.github.io/docs/latest/)
* Docs productos greenbone: [https://docs.greenbone.net/](https://docs.greenbone.net/)
* Arquitectura: [https://forum.greenbone.net/t/about-gvm-20-08-and-21-04-architecture/8449](https://forum.greenbone.net/t/about-gvm-20-08-and-21-04-architecture/8449)

* GVM Kali doc: [https://www.kali.org/tools/gvm/](https://www.kali.org/tools/gvm/)
* Instalación: [https://dannyda.com/2021/12/17/how-to-install-gvm-greenbone-vulnerability-management-openvas-on-kali-linux-2021-4/ ](https://dannyda.com/2021/12/17/how-to-install-gvm-greenbone-vulnerability-management-openvas-on-kali-linux-2021-4/ )
* Solución al problema al crear tareas (Kali purple): [https://forum.greenbone.net/t/scan-config-cant-be-created-failed-to-find-config-daba56c8-73ec-11df-a475-002264764cea/8938/28](https://forum.greenbone.net/t/scan-config-cant-be-created-failed-to-find-config-daba56c8-73ec-11df-a475-002264764cea/8938/28)
* Tutorial uso: [https://securitytrails.com/blog/openvas-vulnerability-scanner](https://forum.greenbone.net/t/scan-config-cant-be-created-failed-to-find-config-daba56c8-73ec-11df-a475-002264764cea/8938/28)

* Solución al problema al crear tareas usando Kali standard (Actualizar feeds y esperar): [https://www.youtube.com/watch?v=DoNaGl1XHYE&t=470s](https://www.youtube.com/watch?v=DoNaGl1XHYE&t=470s)

# Instalación docker

* Doc oficial:
[https://greenbone.github.io/docs/latest/22.4/container/index.html](https://greenbone.github.io/docs/latest/22.4/container/index.html)

* Contenedor alternativo antiguo (no recomendado!!)
[https://github.com/mikesplain/openvas-docker](https://github.com/mikesplain/openvas-docker)
