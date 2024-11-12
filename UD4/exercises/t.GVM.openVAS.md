---
title: Auditoría de seguridad de servidores
---




# Greenbone Vulnerability Manager  (Anteriormente OpenVAS)

Greenbone Vulnerability Management (**GVM**), anteriormente conocido como **OpenVAS**, es una solución de código abierto diseñada para la gestión y evaluación de vulnerabilidades en redes y sistemas informáticos. Proporciona herramientas para identificar, clasificar y gestionar posibles brechas de seguridad, ayudando a las organizaciones a proteger sus infraestructuras contra amenazas conocidas. GVM es ampliamente utilizado por profesionales de seguridad para realizar análisis exhaustivos y asegurar que los sistemas estén actualizados y seguros frente a vulnerabilidades explotables.


![Logo Greenbone](img/GVM/greenbone_logo.png){:style="width: 40%;" class="center"}



# Enunciado

En esta ocasión vamos a intentar usar GVM para auditar varias **máquinas remotas**, pueden ser tanto **linux** como windows. **Para** instalar la herramienta instalaremos una nueva máquina virtual con [Kali Linux](https://www.kali.org/get-kali/#kali-platforms).

## Preparación de máquina objetivo de escaneo

Instala algunos servicios en las máquinas que serán escaneadas antes de comenzar la auditoría. Puedes usar alguna de las máquinas virtuales que ya tengas de otras asignaturas, o puedes configurar una a propósito para realizar las pruebas.

Se sugiere usar una de las máquinas usadas previamente (AlmaLinux). En la máquina que será objetivo de la auditoría se debe instalar algunos servicios, tales como SSH, Apache, MariaDB, etc. De este modo tendremos algo que escanear con GVM.

SSH

```sh
sudo dnf install openssh-server
```

Apache
```sh
sudo dnf install httpd
```

Después de instalar Apache, puedes iniciar y habilitar el servicio con:
```sh
sudo systemctl start httpd
sudo systemctl enable httpd
```


MariaDB
Para instalar el servidor MariaDB:
```sh
sudo dnf install mariadb-server
```

Y luego iniciar el servicio:
```sh
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

> **Revisa la bibliografía antes de empezar.**

## Instala la herramienta GVM

GVM es una herramienta que se ejecuta en una máquina diferente al objetivo a escanear, así que es necesario disponer de otra máquina para poder utilizarla. En esta ocasión se propone usar una máquina virutal en la que instalaremos la distribución  [Kali Linux](https://www.kali.org/get-kali/) para usar la herramienta **GVM**. 

### Requisitos:

Mínimos:
- CPU Cores: 2    
- Random-Access Memory: 4GB    
- Hard Disk: 20GB free


Recomendados:
- CPU Cores: 4    
- Random-Access Memory: 8GB    
- Hard Disk: 60GB free


Para **instalar** y configurar GVM ejecutaremos (**es importante no omitir las actualizaciones del sistema**):

```sh
sudo apt-get update
sudo apt upgrade
sudo apt-get install gvm
```

Para **inicializar** GVM:
```sh
sudo gvm-setup
```

> **Este paso lleva bastante tiempo**. Al terminar la configuración **obtendremos la contraseña del usuario, debemos anotarla**. La salida por la consola será algo parecido a lo siguietne:

```sh

...

[*] Checking Default scanner
[*] Modifying Default Scanner
Scanner modified.

[+] Done
[*] Please note the password for the admin user
[*] User created with password '93215cd9-1f9b-46b1-9d02-093a697cf8d1'.

[>] You can now run gvm-check-setup to make sure everything is correctly configured

```

Podemos comprobar que todo está correctamente instalado mediante el comando:

```sh
sudo gvm-check-setup
```

Puede aparecer algún error, se debe seguir las instrucciones indicadas para corregirlo. En mi caso he obtenido el siguiente error:

```
Step 4: Checking data ... 
        OK: SCAP data found in /var/lib/gvm/scap-data.
        ERROR: CERT data are missing.
        FIX: Run the CERT synchronization script greenbone-feed-sync.
        sudo greenbone-feed-sync --type cert.

 ERROR: Your GVM-23.11.0 installation is not yet complete!

Please follow the instructions marked with FIX above and run this script again.

```

Tal como me indica el error, ejecutaré el siguiente comando para solucionarlo, pero usaré el usuario `_gvm` para evitar problemas de permisos. Las conexiones con el servidor remoto pueden fallar en algún momento, espera unos minutos y vuelve a intentarlo más tarde si te sucede.

```sh
sudo runuser -u _gvm -- greenbone-feed-sync --type cert
```

Ahora, se debe volver a ejecutar el comando `gvm-check-setup` para comprobar si ahora está todo correcto. 


Una vez terminada la instalación y configuración inicial. Necesitamos ejecutar la **actualizaicón de los feeds** con las definiciones de las vulnerabilidades. **No omitir este paso**. Descargará la base de datos de vulnerabilidades y purebas, **tardará un buen rato**.

```sh
sudo runuser -u _gvm -- greenbone-feed-sync
```

<!--
  
Update NVT Feed 

sudo runuser -u _gvm -- greenbone-nvt-sync 

Update SCAP Feed 

sudo runuser -u _gvm -- greenbone-feed-sync --type SCAP 

Update CERT Feed 

 sudo runuser -u _gvm -- greenbone-feed-sync --type CERT 

 Update gvmd DATA Feed 
 
 sudo runuser -u _gvm -- greenbone-feed-sync --type GVMD_DATA
-->

<!-- antes era:
gvm-feed-update -h
-->

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

![GVM login page](img/GVM/GVM_1_login.png){:style="width: 70%;" class="center"}

Recuerda que la cuenta es `admin`y la contraseña se generó al ejecutar el comando `sudo gvm-setup`.



Para detener el servicio (cuando no se quiera seguir utilizando):

```sh
 sudo gvm-stop
```


Una vez instalado y configurado ya  puedes usar la herramienta para escanear las vulnerabilidades de la máquina objetivo.

## Utilización de GVM

Antes de iniciar el primer escaneo, Greenbone necesita analizar las fuentes de vulnerabilidades y almacenarlas en la base de datos PostgreSQL de gvmd; de lo contrario, no podrá iniciar o completar los escaneos sin errores. Este proceso se inicia durante la etapa de configuración, pero **generalmente toma desde unos pocos minutos hasta varias horas para completarse**, dependiendo de los recursos de tu sistema.

Kali Linux muestra un indicador de CPU en la la barra de tareas, mientras observes actividad tendrás que esperar.

## Crear una tarea

Debemos usar el menú `scans->tasks`, crearemos una nueva tarea utilizando el wizard y la ejecutaremos.
Podemos escanear varias máquinas.

![GVM login page](img/GVM/GVM_2_task_wizard.png){:style="width: 70%;" class="center"}



## Tareas a realizar

> 1. **Instala GVM**    
>     - Proporciona evidencias de la instalación, como capturas de pantalla del proceso o comandos ejecutados.
> 2. **Escanea un mínimo de dos máquinas**    
>     - Las máquinas pueden tener diferentes sistemas operativos o servicios instalados.
>     - Asegúrate de contar con autorización para escanear estas máquinas.
> 3. **Muestra los resultados de los escaneos realizados**    
>     - Presenta los informes generados por GVM.
>     - Incluye capturas de pantalla y explica brevemente los hallazgos.
> 4. **Revisa los informes y comenta los problemas encontrados**    
>     - Analiza las vulnerabilidades detectadas.
>     - Propón soluciones o medidas de mitigación para cada problema identificado.

## Advertencias y consideraciones éticas

- **Importante:** Asegúrate de tener autorización para realizar escaneos de vulnerabilidades en las máquinas objetivo. Realizar escaneos sin permiso puede tener implicaciones legales y éticas.

## Posibles Problemas 

Es posible encontrar problemas durante la instalación y configuración. Debes haber seguido los pasos descritos anteriormente:

- Haber actualizado el sistema.
- Haber comprobado la corrección de la instalación.
- Actualizar los feeds.
- Arrancar GVM y **esperar un buen rato** para que se importen las nuevas vulnerabilidades.

Después sigue las indicaciones para solucionar problemas que econtrarás en el siguiente enlace: [https://greenbone.github.io/docs/latest/22.4/kali/troubleshooting.html](https://greenbone.github.io/docs/latest/22.4/kali/troubleshooting.html)

### Problema 1: Falta de pestañas en interfaz web

No has actualizado los paquetes antes de instalar, actualiza y sigue las instrucciones de este enlace:
https://greenbone.github.io/docs/latest/22.4/kali/troubleshooting.html

Ve comprobando si está todo correcto con el comando:


```sh
sudo gvm-check-setup
```

### Problema 2: Problemas al crear una tarea

Si alguna vez te encuentres con un problema como el siguiente:

![GVM login page](img/GVM/GVM_3_task_problem.png){:style="width: 70%;" class="center"}


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
sudo runuser -u _gvm -- gvmd --modify-setting  78eceaec-3385-11ea-b237-28d24461215b --value <uid>
```

Con el resultado del ejemplo anterior el comando quedaría de la siguiente forma:

`sudo runuser -u _gvm -- gvmd --modify-setting 8eceaec-3385-11ea-b237-28d24461215b --value ea55b1d6-1d15-4a0a-891c-64cfea123276`

Reiniciamos GVM:

```sh
sudo gvm-stop
sudo gvm-start
```



# Bibliografía

* Greenbone Community Edition: [Documentación](https://greenbone.github.io/docs/latest/)
	*  [Arquitectura OpenVAS](https://greenbone.github.io/docs/latest/background.html)
* Desarrollador Greenbone:  [https://www.greenbone.net/](https://www.greenbone.net/)
	* Docs productos greenbone: [https://docs.greenbone.net/](https://docs.greenbone.net/)
- Foros [Greenbone Community](https://forum.greenbone.net/)


### Instalación Usando Kali Linux
- Instalación de Kali en [VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)
* GVM Kali Linux [Install Guide](https://greenbone.github.io/docs/latest/22.4/kali/index.html)
* GVM Kali Linux [Troubleshooting](https://greenbone.github.io/docs/latest/22.4/kali/troubleshooting.html)
* GVM Kali Linux [Tool Documentation](https://www.kali.org/tools/gvm/)
* GVM[ Feed Synchronization](https://greenbone.github.io/docs/latest/22.4/source-build/feed-sync.html)

### Instalación docker

*  Greenbone Community Containers:[https://greenbone.github.io/docs/latest/22.4/container/index.html](https://greenbone.github.io/docs/latest/22.4/container/index.html)

### Problemas

- Faltan opciones en WebUI: [https://forum.greenbone.net/t/gui-not-showing-all-options-kali/19215/10](https://forum.greenbone.net/t/gui-not-showing-all-options-kali/19215/10)
* Solución al problema al crear tareas (Kali purple): [https://forum.greenbone.net/t/scan-config-cant-be-created-failed-to-find-config-daba56c8-73ec-11df-a475-002264764cea/8938/28](https://forum.greenbone.net/t/scan-config-cant-be-created-failed-to-find-config-daba56c8-73ec-11df-a475-002264764cea/8938/28)
* Solución al problema al crear tareas usando Kali standard (Actualizar feeds y esperar): [https://www.youtube.com/watch?v=DoNaGl1XHYE&t=470s](https://www.youtube.com/watch?v=DoNaGl1XHYE&t=470s)
- [GitHub greenbone-feed-sync](https://github.com/greenbone/greenbone-feed-sync)

## Vídeo Instalación GVM

<iframe width="560" height="315" src="https://www.youtube.com/embed/_eLI8XuXf4I?si=q9foJXGcl3GKEuL4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
## Vídeo utilización GVM

<iframe width="560" height="315" src="https://www.youtube.com/embed/Q-O4lPCGEXw?si=t5zfUQRyRq0dgG50" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
